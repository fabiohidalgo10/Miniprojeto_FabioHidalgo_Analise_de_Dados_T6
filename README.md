# Análise Exploratória de Dados - Base Varejo

Mini-Projeto Avaliativo - Módulo 1 - Semana 07 - Turma **Analise_de_Dados_T6**

## 📌 Sobre o projeto

Este repositório contém uma Análise Exploratória de Dados (AED) aplicada a uma
base real de compras de varejo (`Base_Varejo.csv`), com o objetivo de:

- identificar problemas de qualidade nos dados (nulos, duplicatas, inconsistências);
- limpar e padronizar a base;
- gerar estatísticas descritivas;
- explorar padrões de compra via agrupamentos;
- comunicar os principais insights encontrados.

Base de dados original (Kaggle):
https://www.kaggle.com/datasets/namespaiva/base-varejo/data

## 🗂️ Estrutura do repositório

```
├── Base_Varejo.csv                  # base original (bruta)
├── Miniprojeto_NomeAluno_T6.py      # script da AED (comentado por bloco)
├── df_limpo.csv                     # base já limpa, gerada pelo script
├── README.md                        # este arquivo
└── README_NomeDoAluno_Turma.md      # instruções rápidas de execução
```

## 🔎 Dicionário de dados (colunas originais)

| Coluna      | Descrição                                              |
|-------------|---------------------------------------------------------|
| DATA        | Data da compra (dd/mm/aaaa)                              |
| CO_ID       | Identificador da **compra** (repete para cada item dela) |
| CL_ID       | Identificador do cliente                                 |
| CL_GENERO   | Gênero do cliente (M/F)                                  |
| CL_EC       | Estado civil do cliente (código)                         |
| CL_FHL      | Número de filhos do cliente                              |
| CL_SEG      | Segmento do cliente (A/B/C)                               |
| PR_ID       | Identificador do produto                                 |
| PR_CAT      | Categoria do produto (categoria ausente = "#N/D")        |
| PR_NOME     | Nome do produto                                           |

> Importante: cada **linha** da base representa um **item comprado**, não uma
> compra inteira. A coluna `CO_ID` é o identificador da compra e deve ser
> usada para agrupar os itens de um mesmo carrinho.

## 🧪 Problemas identificados na base

1. **4 colunas totalmente vazias** (`Unnamed: 10` a `Unnamed: 13`), causadas por
   `;` sobrando no final de cada linha do CSV original.
2. **96.553 linhas duplicadas** (registro idêntico em todas as colunas).
3. **3.650 itens com categoria ausente**, representada pela string `"#N/D"`
   em vez de um nulo tradicional.
4. A coluna `DATA` estava como **texto**, não como data.

## 🧹 Limpeza aplicada

| # | Ação | Escolha e justificativa |
|---|------|--------------------------|
| 1 | Remover colunas 100% vazias | Não há nenhuma informação para imputar; são resíduo de exportação. |
| 2 | Imputar categoria `"#N/D"` → `"Sem Categoria"` | O item vendido é uma informação válida; excluir a linha jogaria fora vendas reais. |
| 3 | Remover linhas duplicadas | Uma duplicata exata é o mesmo item contado duas vezes, distorcendo contagens e estatísticas. |
| 4 | Converter `DATA` de string para `datetime` | Necessário para análises temporais (mês, sazonalidade). |

## 📊 Principais análises

- Estatísticas descritivas da coluna **número de filhos do cliente** (`CL_FHL`):
  média, mediana, desvio padrão, moda, mínimo, máximo e quartis (calculadas
  sobre clientes únicos, para não repetir o mesmo cliente várias vezes).
- Agrupamento 1: itens vendidos por **gênero x categoria de produto**.
- Agrupamento 2: itens vendidos por **mês x segmento de cliente**.
- Agrupamento 3 (extra): top 10 produtos mais vendidos por categoria.

## 💡 Conclusões / Insights

1. A base tem 18.471 compras distintas e 733.447 itens vendidos após a limpeza
   (96.553 linhas duplicadas removidas).
2. A categoria **ALIMENTOS** concentra o maior volume de itens vendidos.
3. O gênero **F** concentra a maior parte das compras registradas.
4. Há variação mensal no volume de vendas, sugerindo sazonalidade no
   comportamento de compra ao longo do ano.
5. Em média os clientes têm ~1,1 filho (mediana 0), com desvio padrão de 1,41 -
   perfil familiar relativamente concentrado em famílias pequenas.
6. Mesmo após o tratamento, os 3.650 registros que vieram sem categoria
   original merecem investigação na origem do dado, para evitar recorrência.

## 🧠 Reflexão teórica: ETL e qualidade de dados

Este projeto reproduz, em pequena escala, um pipeline de **ETL (Extract,
Transform, Load)**:

- **Extract (Extração):** leitura do arquivo `Base_Varejo.csv` com `pandas`,
  já identificando o separador correto (`;`) e checando a estrutura (linhas,
  colunas, tipos).
- **Transform (Transformação):** é a etapa mais sensível do processo. Antes
  de qualquer análise, é preciso **auditar** a qualidade dos dados (nulos,
  duplicatas, valores fora do padrão) e só então decidir, com justificativa,
  se cada problema deve ser corrigido por **remoção** ou por **imputação**.
  Decisões de limpeza feitas sem critério (por exemplo, remover duplicatas
  sem entender se são erradas ou legítimas) podem distorcer completamente os
  insights finais.
- **Load (Carga):** a base tratada (`df_limpo.csv`) fica pronta para ser
  consumida por uma etapa seguinte do pipeline - um dashboard de BI, um
  modelo estatístico ou uma nova rodada de análise.

A qualidade de dados não é uma etapa opcional: é o que garante que as
estatísticas e os agrupamentos gerados reflitam a realidade do negócio, e não
ruídos de captura/exportação do sistema de origem. Um dado "bonito" nas
médias, mas construído sobre uma base suja, produz decisões de negócio
erradas com aparência de segurança estatística.

## ▶️ Como executar

Veja o arquivo `README_NomeDoAluno_Turma.md`.
