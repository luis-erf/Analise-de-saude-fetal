# Análise de Saúde Fetal — Reproduzir exclusivamente no Google Colab a partir do repositório baixado

Este README descreve o fluxo específico para reproduzir os notebooks Jupyter no Google Colab usando os arquivos obtidos diretamente do repositório GitHub (baixando/clonando o repositório no seu computador) e, em seguida, *fazendo upload dos notebooks e dos arquivos de dataset para o Colab*. Todas as instruções abaixo são pensadas apenas para execução no Colab.

---

## Fluxo geral resumido
1. Baixe o repositório do GitHub no seu computador.
2. No Google Colab, faça upload do notebook (.ipynb) que deseja executar (a partir dos arquivos que você baixou).
3. No Colab, faça upload dos arquivos de dataset correspondentes (os arquivos do repositório que você baixou).
4. Execute as células do notebook no Colab na ordem indicada no próprio notebook.

---

## 1) Baixar o repositório (no seu computador)
Opções:

- Clonar via git:
```bash
git clone https://github.com/luis-erf/Analise-de-saude-fetal.git
```

- Ou baixar ZIP:
  - Acesse a página do repositório e clique em "Code" → "Download ZIP".
  - Extraia o ZIP em uma pasta local.

Após baixar, você terá a estrutura com os notebooks e arquivos de dataset e pastas auxiliares.

---

## 2) Abrir notebooks no Google Colab (a partir dos arquivos baixados)

Opções para abrir um notebook do seu computador no Colab:

- No Colab: File > Open notebook > Upload → selecione o arquivo `.ipynb` que você baixou do repositório.

- Alternativa via upload dentro do notebook (útil se quiser programaticamente enviar vários arquivos):
```python
from google.colab import files
uploaded = files.upload()  # aparecerá um seletor para escolher arquivos .ipynb, .csv etc.
```
Observação: Para notebooks é mais simples usar "Open notebook > Upload" do menu, porque o Colab abre o .ipynb automaticamente.

---

## 3) Enviar os arquivos de dataset para o Colab (a partir dos baixados)

- Pelo diálogo de upload (preferido para poucos arquivos):
  - No Colab: File > Open notebook > Upload (ou usar `files.upload()` em uma célula).
  - Selecione os arquivos de dataset (ex.: `dataset_original.csv`, `dataset_tratado.csv`) da pasta onde você extraiu o repositório.

Exemplo de upload via código:
```python
from google.colab import files
uploaded = files.upload()  # selecione dataset_original.csv (ou os arquivos que desejar)
```


## 4) Ordem de execução recomendada dos notebooks
(Siga esta ordem abrindo cada notebook no Colab e fazendo upload dos datasets correspondentes)

1. 01_EDA.ipynb
   - Upload: `dataset_original.csv` (do repositório baixado).
   - Objetivo: inspeção e estudo das variáveis.

2. 02_tratamento.ipynb
   - Upload: `fetal_healt.csv`, que está na pasta Dataset original.
   - Executar limpeza e engenharia de features.

3. 03_modelagem.ipynb
   - Upload: `fetal_health_test_multi.csv`, `fetal_health_train_multi.csv`, que está na pasta Dataset tratado

---

Autor: luis-erf  
Data: 2026-01-30