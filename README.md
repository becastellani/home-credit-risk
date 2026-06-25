# Home Credit Default Risk

Previsão de risco de inadimplência em concessão de crédito, com base nos dados da competição [Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk) do Kaggle.

Projeto desenvolvido em 4 sprints — da análise exploratória ao modelo final interpretável.

## Pergunta de negócio

No momento da solicitação de crédito, quais clientes têm risco de inadimplência alto o suficiente para justificar negação automática ou revisão manual, considerando que rejeitar um bom pagador tem custo de oportunidade e aprovar um mau pagador tem custo de perda financeira direta?

## Contexto técnico

A base principal (`application_train.csv`) contém ~307k solicitações de crédito com 122 atributos disponíveis no momento da decisão. O target (`TARGET`) é binário: 0 = cliente pagou normalmente, 1 = cliente teve dificuldades de pagamento nas parcelas iniciais.

A distribuição de classes é desbalanceada (~92% negativos / ~8% positivos). Um modelo que prediz a classe majoritária para todos os casos entrega 92% de accuracy sem nenhum valor preditivo real — por isso accuracy não é a métrica de interesse. O modelo reflete o tradeoff assimétrico entre **falso negativo** (aprovar inadimplente → perda financeira direta) e **falso positivo** (negar bom pagador → custo de oportunidade), e é explicável para fins de auditoria regulatória (Basel III).

## Resultado final

| Métrica | Valor no conjunto de teste |
|---|---|
| **Modelo final** | HistGradientBoostingClassifier (`class_weight='balanced'`) |
| **AUC-ROC** | **0,763** (meta > 0,70 atingida) |
| **Recall — classe inadimplente** | 68,9% (3.421 de 4.965 inadimplentes detectados) |
| **F1-Macro** | 0,543 |

Métrica principal: **AUC-ROC** (robusta ao desbalanceamento). A escolha do modelo seguiu uma regra objetiva definida antes da avaliação no teste: HistGBT só seria preferido à Regressão Logística se superasse a AUC em ≥ 0,005 — caso contrário, a interpretabilidade da LR prevaleceria.

## Estrutura do projeto

```
home-credit-risk/
├── data/
│   ├── raw/                       # dataset original (não versionado — baixar do Kaggle)
│   └── processed/                 # dados transformados
├── notebooks/
│   ├── sprint1_eda.ipynb          # Sprint 1 — análise exploratória
│   ├── sprint2.ipynb              # Sprint 2 — pré-processamento e feature engineering
│   ├── sprint3.ipynb              # Sprint 3 — modelagem, comparação e ajuste
│   ├── sprint4.ipynb              # Sprint 4 — análise de erros, SHAP, hipóteses, limitações
│   └── modelo_projeto.pkl         # modelo final serializado (gerado pela Sprint 3)
├── slides/
│   ├── apresentacao_sprint4.pptx  # apresentação final
│   └── build_deck.py              # script gerador da apresentação
├── docs/
│   └── sprint1_findings.md
├── .gitignore
├── README.md
└── requirements.txt
```

## Sprints

| Sprint | Escopo | Entregável |
|--------|--------|-----------|
| 1 | Definição do problema e EDA | `notebooks/sprint1_eda.ipynb` |
| 2 | Pré-processamento e feature engineering (pipeline em 8 etapas) | `notebooks/sprint2.ipynb` |
| 3 | Modelagem: 4 modelos, cross-validation, GridSearch/RandomizedSearch, avaliação final | `notebooks/sprint3.ipynb` + `modelo_projeto.pkl` |
| 4 | Análise de erros (foco em FN), interpretabilidade (SHAP), revisão das hipóteses, limitações | `notebooks/sprint4.ipynb` + `slides/` |

## Como reproduzir

1. **Instalar dependências:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Baixar o dataset:** o arquivo `application_train.csv` não está no repositório (≈158 MB). Faça o download em [Kaggle — Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk/data) e coloque em `data/raw/`.

3. **Executar os notebooks na ordem das sprints.** A ordem importa: a Sprint 3 treina e salva `modelo_projeto.pkl`, que é carregado pela Sprint 4.
   ```
   sprint1_eda.ipynb → sprint2.ipynb → sprint3.ipynb → sprint4.ipynb
   ```

> **Nota:** `data/raw/`, `data/processed/` e arquivos `*.pkl` são ignorados pelo Git (ver `.gitignore`). O modelo serializado é regenerado ao executar a Sprint 3.

## Dataset

Fonte: [Kaggle — Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk/data)
