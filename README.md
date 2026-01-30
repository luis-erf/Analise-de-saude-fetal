# Análise de Saúde Fetal — Reproduzir exclusivamente no Google Colab a partir do repositório baixado

Este README descreve o fluxo específico para **reproduzir os notebooks Jupyter no Google Colab** usando os arquivos obtidos diretamente do repositório GitHub (baixando/clonando o repositório no seu computador) e, **adicionalmente**, como **utilizar um modelo já treinado** sem necessidade de re-treinamento.

Todas as instruções abaixo são pensadas **apenas para execução no Google Colab**.

---

## Fluxo geral resumido

1. Baixe o repositório do GitHub no seu computador.
2. No Google Colab, faça upload do notebook (.ipynb) que deseja executar.
3. No Colab, faça upload dos arquivos de dataset correspondentes.
4. Execute os notebooks na ordem indicada.
5. (Opcional) Utilize o **modelo treinado** para gerar previsões em novos dados, sem re-treinamento.

---

## 1) Baixar o repositório (no seu computador)

Opções:

* Clonar via git:

```bash
git clone https://github.com/luis-erf/Analise-de-saude-fetal.git
```

* Ou baixar ZIP:

  * Acesse a página do repositório e clique em **Code → Download ZIP**.
  * Extraia o ZIP em uma pasta local.

Após baixar, você terá a estrutura com os notebooks, datasets e arquivos auxiliares.

---

## 2) Abrir notebooks no Google Colab (a partir dos arquivos baixados)

Opções para abrir um notebook do seu computador no Colab:

* No Colab: **File → Open notebook → Upload** → selecione o arquivo `.ipynb` baixado.

* Alternativa via upload por código (menos comum para notebooks):

```python
from google.colab import files
uploaded = files.upload()
```

> Observação: para notebooks, a opção **Open notebook → Upload** é a mais simples.

---

## 3) Enviar os arquivos de dataset para o Colab

* Pelo diálogo de upload:

  * No Colab: **Files → Upload** ou via `files.upload()`.
  * Selecione os arquivos de dataset presentes no repositório baixado.

Exemplo via código:

```python
from google.colab import files
uploaded = files.upload()
```

---

## 4) Ordem de execução recomendada dos notebooks

Abra cada notebook no Colab, faça upload dos datasets indicados e execute as células na ordem.

### 4.1) `01_EDA.ipynb`

* Upload: `dataset_original.csv`
* Objetivo: análise exploratória e entendimento das variáveis.

### 4.2) `02_tratamento.ipynb`

* Upload: `fetal_health.csv` (pasta *Dataset original*)
* Objetivo: limpeza, tratamento e engenharia de features.

### 4.3) `03_modelagem.ipynb`

* Upload: `fetal_health_train_multi.csv` e `fetal_health_test_multi.csv` (pasta *Dataset tratado*)
* Objetivo: treinamento, validação e avaliação dos modelos.

---

## 5) Utilizar o modelo já treinado (sem re-treinar)

Esta etapa demonstra como **carregar o modelo pronto** e aplicá-lo diretamente em novos dados, apenas para inferência.

### 5.1) Upload dos arquivos necessários

No Google Colab, faça upload dos seguintes arquivos (presentes no repositório):

* `svm_fetal_health_best.pkl` → modelo treinado
* `robust_scaler.pkl` → scaler ajustado no treino

---

### 5.2) Carregar os dados do exame de cardiotocografia

Exemplo:

```python
# Instalar dependência, se necessário
# pip install kagglehub[pandas-datasets]

import kagglehub
from kagglehub import KaggleDatasetAdapter

file_path = "fetal_health.csv"

# Carregar dataset original do Kaggle
df = kagglehub.load_dataset(
    KaggleDatasetAdapter.PANDAS,
    "andrewmvd/fetal-health-classification",
    file_path,
)

print("First 5 records:")
print(df.head())
```

---

### 5.3) Carregar o modelo e o scaler

```python
import joblib
import numpy as np

model = joblib.load("svm_fetal_health_best.pkl")
scaler = joblib.load("robust_scaler.pkl")
```

---

### 5.4) Pré-processamento

```python
# Amostra de dados para teste
df_sample = df.sample(n=100, random_state=42)

# Features removidas no processo de modelagem
features_FEliminar = [
    "percentage_of_time_with_abnormal_long_term_variability",
    "mean_value_of_short_term_variability",
]
features_HEliminar = [
    "histogram_median", "histogram_mean", "histogram_mode",
    "histogram_number_of_peaks", "histogram_number_of_zeroes",
    "histogram_min", "histogram_max", "histogram_width",
]

# Remover features não utilizadas
df_sample = df_sample.drop(columns=features_FEliminar)
df_sample = df_sample.drop(columns=features_HEliminar)

# Conversão multiclasse → binária (classe 3 → 2)
df_sample['fetal_health'] = df_sample['fetal_health'].replace(3, 2)

X = df_sample.drop('fetal_health', axis=1)
y = df_sample['fetal_health']
```

---

### 5.5) Escalonamento e inferência

```python
from sklearn.metrics import classification_report

print("Aplicando scaler...")
X_test_scaled = scaler.transform(X)

print("Gerando previsões...")
y_pred = model.predict(X_test_scaled)

print("\nRelatório de Classificação (sem re-treinar):")
print(classification_report(y, y_pred))
```

---