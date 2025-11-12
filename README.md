# Healthcare Data Analysis

Este projeto tem como objetivo realizar uma análise exploratória e tratamento de dados do conjunto de dados de saúde (`healthcare_dataset.csv`) utilizando as bibliotecas **pandas**, **numpy** e **matplotlib** em Python.

## 📁 Estrutura

- `index.ipynb` — Notebook Jupyter/Colab com todo o pipeline de ingestão, limpeza, exploração e visualização dos dados.
- `healthcare_dataset.csv` — Arquivo CSV com os dados de saúde utilizados para análise.
- `README.md` — Este arquivo explicativo que você está lendo.

## ✅ Funcionalidades

### 1. Importação e cópia dos dados

- Carrega o arquivo `healthcare_dataset.csv` com `pd.read_csv`.
- Cria uma cópia do DataFrame original (`df_clean = healthcare_df.copy()`) para manter os dados originais intactos.

### 2. Definição de funções de tratamento de dados

São definidas funções gerais para tratar o dataset de modo automatizado:

- `tratar_dados_ausentes(data)`
  - Converte as colunas **“Room Number”**, **“Billing Amount”** e **“Age”** para tipo numérico, usando `pd.to_numeric(..., errors='coerce')`.
  - Verifica se há colunas do tipo objeto (`object`) com valores nulos; caso haja, preenche todos esses valores com o rótulo `"Valor não definido"`.
- `tratar_fatura_negativa(data)`
  - Identifica valores da coluna **“Billing Amount”** menores que zero (faturas negativas) e as substitui por `NaN`.
  - Verifica se a coluna ficou com valores nulos e, se sim, preenche com a média da coluna.
- `tratar_data(data)`
  - Converte as colunas **“Date of Admission”** e **“Discharge Date”** para tipo `datetime`, usando `pd.to_datetime(..., errors='coerce')`.
  - Preenche valores ausentes nessas colunas com a data atual (formato “YYYY-MM-DD”).
- `tratar_nomes(data)`
  - Para todas as colunas do tipo objeto (`object`), padroniza os valores para “Title Case” (primeira letra maiúscula), usando `str.title()`.
- `identificar_outliers(data)`
  - Computa os quartis Q1 e Q3, o IQR (Q3–Q1), e define limites inferior e superior (Q1 – 1.5×IQR, Q3 + 1.5×IQR).
  - Retorna os valores que caem fora desses limites — ou seja, possíveis outliers.
- `normalizar_dado(data)`
  - Aplica a normalização min-máx: \((x - \min)/( \max - \min)\), retornando uma nova série entre 0-1.

### 3. Aplicação dos tratamentos

As funções são aplicadas em sequência sobre `df_clean`:

- `tratar_data(df_clean)`
- `tratar_nomes(df_clean)`
- `tratar_dados_ausentes(df_clean)`
- `tratar_fatura_negativa(df_clean)`  
  Dessa forma, o DataFrame fica limpo, com tipos corretos, sem valores inválidos ou negativos na fatura, e com formatação padronizada.

### 4. Criação de novas colunas para análise

- `Days in Hospital` — calcula o número de dias que o paciente ficou internado, a partir de `Discharge Date − Date of Admission`.
- `Billing Normalized` — normaliza os valores da fatura (coluna **“Billing Amount”**) para faixa 0-1.
- `Age Bracket` — cria faixas etárias usando `pd.cut`, com os intervalos: 13-18, 18-40, 40-65, 65+.
- `Hospital Length of Stay` — agrupa os pacientes em categorias de tempo de internação: “1-15”, “16-30”, “30+” dias.

### 5. Agrupamentos e métricas

- Média de `Billing Normalized` por seguradora (`Insurance Provider`).
- Média de custo por condição médica (`Medical Condition`).
- Frequência relativa de doenças por faixa etária.

### 6. Visualizações

Diversos gráficos para explorar os dados de diferentes perspectivas:

- Gráfico de barras do **custo médio por faixa etária** (`Age Bracket`).
- Gráfico de barras empilhadas de **medicação por condição médica** (frequência de cada `Medication` dentro de cada `Medical Condition`).
- Gráfico de barras do **custo médio por seguradora**.
- Gráfico de pizza da **distribuição de pacientes por seguradora**.
- Subplots para:
  - distribuição de gênero;
  - proporção de gêneros por seguradora.
- Boxplot e scatterplot da **diferença de preços por gênero**.
- Barras empilhadas do **tipo de admissão por condição médica**.
- Pizza + barras do **resultado dos testes médicos** gerais + por condição.
- Boxplot + scatterplot da **relação entre tempo de internação (dias) e custo médio**.
- Barras empilhadas da **categoria de tempo de internação (“Length of Stay”) por condição médica**.

### 7. Possíveis melhorias / observações

- A função `identificar_outliers()` foi definida, mas não aparece aplicada diretamente no fluxo; poderia ser usada para filtrar ou marcar registros extremos.
- O preenchimento de datas ausentes com a data atual pode introduzir viés analítico; idealmente, ausências deveriam ser tratadas de forma contextual.
- A normalização (`Billing Normalized`) facilita comparações, mas os valores absolutos ainda são usados para métricas médias — é bom explicar isso no relatório.
- A visualização de gênero vs custo poderia incluir teste estatístico (ex: t-teste) para ver se a diferença é significativa.
- Para reutilização futura, o pipeline de tratamento poderia ser colocado em uma função ou classe, ou dentro de um módulo para tornar o código mais modular e reutilizável.

## 🚀 Como usar

1. Clone o repositório:
   ```bash
   git clone https://github.com/alanmatheusc/DEVinHouse-Clamed_HealthcareDataAnalysis.git
   ```
