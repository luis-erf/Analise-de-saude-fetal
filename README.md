# Executando o repositório localmente (sem Google Colab)

Este documento explica como preparar um ambiente local para executar os notebooks presentes neste repositório, evitando o uso do Google Colab.

## 1) Escolha do ambiente
Opções recomendadas:
- Usar Conda / Miniconda (recomendado para ciência de dados)
- Ou usar um virtualenv / venv com pip

### A) Criar ambiente Conda (recomendado)
1. Instale Miniconda/Conda se ainda não tiver: https://docs.conda.io/en/latest/miniconda.html
2. No diretório do repositório, execute:
   ```
   conda env create -f environment.yml
   conda activate fetal_health_env
   ```
3. Registre o kernel (opcional, para usar nos notebooks):
   ```
   python -m ipykernel install --user --name fetal_health_env --display-name "Python (fetal_health_env)"
   ```

### B) Usar virtualenv / pip
1. Crie e ative o virtualenv:
   ```
   python3 -m venv .venv
   source .venv/bin/activate    # macOS / Linux
   .venv\Scripts\activate       # Windows (PowerShell)
   ```
2. Instale as dependências:
   ```
   pip install --upgrade pip
   pip install -r requirements.txt
   ```
3. Registre o kernel (opcional):
   ```
   python -m ipykernel install --user --name fetal_health_local --display-name "Python (fetal_health_local)"
   ```

## 2) Ajustes nos notebooks (substituir trechos Colab)
Os notebooks do repositório utilizam trechos específicos do Colab (ex.: `from google.colab import files`, `files.upload()` e células com `!pip install ...`). Para execução local, faça as alterações abaixo:

- Remova ou comente qualquer célula com `!pip install ...` (já estão instaladas pelo environment/requirements).
- Substitua blocos que usam `files.upload()` por carregamento direto com `pd.read_csv()` apontando para o caminho local do arquivo.

Exemplo de alteração:
Antes (Colab):
```python
from google.colab import files
uploaded = files.upload()
for filename in uploaded.keys():
    os.rename(filename, 'fetal_health_train_multi.csv')
df_train = pd.read_csv('fetal_health_train_multi.csv')
```

Depois (local):
```python
# Coloque o arquivo fetal_health_train_multi.csv no mesmo diretório do notebook
import os
import pandas as pd

dataset_path = 'fetal_health_train_multi.csv'
if not os.path.exists(dataset_path):
    raise FileNotFoundError(f"O arquivo {dataset_path} não foi encontrado. Coloque-o na pasta do notebook.")
df_train = pd.read_csv(dataset_path)
```

- Substitua `from google.colab import files` por importações locais/nenhuma.
- Caso exista uso de `drive.mount(...)`, remova e coloque instruções para copiar os arquivos localmente.

## 3) Executar os notebooks
- Inicie Jupyter Lab (ou Notebook):
  ```
  jupyter lab
  # ou
  jupyter notebook
  ```
- Abra os notebooks (.ipynb) e selecione o kernel correspondente (ex.: "Python (fetal_health_env)").
- Execute células sequencialmente.

## 4) Observações sobre versões
- O arquivo `requirements.txt` e `environment.yml` trazem versões fixas para reprodutibilidade. Caso encontre conflito com seu sistema, ajuste as versões conforme necessário.
- Se preferir que eu gere arquivos com versões mínimas (ranges) em vez de pins exatos, eu também posso gerar.

## 5) Checklist rápido para rodar localmente
- [ ] Criar/ativar ambiente (Conda ou venv)
- [ ] Instalar dependências (conda env create -f environment.yml OU pip install -r requirements.txt)
- [ ] Colocar os CSVs usados pelos notebooks na mesma pasta ou ajustar os caminhos
- [ ] Remover/editar células Colab conforme instruído
- [ ] Abrir Jupyter Lab / Notebook e selecionar o kernel correto
