# Home Credit Default Risk

Análise de risco de inadimplência com base nos dados da competição [Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk) do Kaggle.

## Pergunta de negócio

No momento da solicitação de crédito, quais clientes têm risco de inadimplência alto o suficiente para justificar negação automática ou revisão manual, considerando que rejeitar um bom pagador tem custo de oportunidade e aprovar um mau pagador tem custo de perda financeira direta?

## Contexto técnico

A base principal (`application_train.csv`) contém ~307k solicitações de crédito com 122 atributos disponíveis no momento da decisão. O target (`TARGET`) é binário: 0 = cliente pagou normalmente, 1 = cliente teve dificuldades de pagamento nas parcelas iniciais.

A distribuição de classes é desbalanceada (~92% negativos / ~8% positivos). Um modelo que prediz a classe majoritária para todos os casos entrega 92% de accuracy sem nenhum valor preditivo real — por isso accuracy não é a métrica de interesse. O modelo final precisará refletir o tradeoff assimétrico entre falso negativo (aprovar inadimplente) e falso positivo (negar bom pagador), e ser explicável para fins de auditoria regulatória.

## Estrutura do projeto

```
home-credit-risk/
├── data/
│   ├── raw/              # dataset original — não versionar
│   └── processed/        # dados transformados para modelagem
├── notebooks/
│   └── sprint1_eda.ipynb # análise exploratória — Sprint 1
├── docs/
│   └── sprint1_findings.md
├── .gitignore
├── README.md
└── requirements.txt
```

## Sprints

| Sprint | Escopo |
|--------|--------|
| 1 | Definição do problema e EDA |
| 2 | Pré-processamento e feature engineering |
| 3 | Modelagem e avaliação |
| 4 | Interpretabilidade e relatório final |

## Dataset

Fonte: [Kaggle — Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk/data)

O arquivo `application_train.csv` não está incluído no repositório. Faça o download manualmente e coloque em `data/raw/`.

## Requisitos

```bash
pip install -r requirements.txt
```
