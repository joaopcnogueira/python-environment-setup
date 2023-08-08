# Setup Ambiente Python

# Motivação

Imagine que você ficou responsável por realizar uma análise exploratória sobre dados de churn de uma empresa que solicitou os serviços de consultoria da Datarisk. Você já recebeu os dados e quer começar a trabalhar com eles, carregando-os no Pandas e iniciando as análises. Para tal, você precisará realizar o setup de um ambiente python específico para esse projeto, para que não gere conflitos com as dependências e pacotes de outros projetos.

# Pré-Requisitos

1. Sistema Operacional Linux (de preferência Ubuntu), ou
2. Windows Subsystem for Linux (WSL) com Ubuntu instalado, ou
3. Sistema Operacional MacOS, ou
4. ~~Windows~~

<aside>
✅ O Windows pode sim ser utilizado, embora o setup seja um pouco diferente do que é feito em sistemas unix-based (linux e macos). Para mais informações, consultar o google.

</aside>

1. Verifique se tem o python instalado com versão maior ou igual a 3.7

```bash
python --version
```

1. Tenha um editor de código instalado. Recomendamos utilizar o VSCode.

# Passo a Passo

1. Abra o terminal
2. Navegue até a pasta em que você costuma salvar seus projetos utilizando o comando ([como navegar em pastas pelo terminal](https://medium.com/@paulodalves/como-navegar-pelas-pastas-no-terminal-39be96a4c387))
3. Crie a pasta do projeto em que você irá trabalhar digitando o comando abaixo no terminal

```bash
mkdir churn-data-analysis
```

1. Entre na pasta do projeto

```bash
cd churn-data-analysis
```

1. Crie o ambiente virtual do python

```bash
python -m venv .venv
```

1. Ative o ambiente virtual do Python

```bash
source -m venv .venv
```

1. Verifique se de fato o ambiente foi ativado solicitando qual python está sendo utilizado com o seguinte comando

```bash
which python
# output: /Users/joaonogueira/churn-data-analysis/.venv/bin/python
```

<aside>
❗ Perceba no output do comando acima que o python que está sendo utilizado é o python que está dentro da pasta `.venv` que por sua vez está dentro da pasta `churn-data-analysis` que foi criada para o projeto.

</aside>

1. Com o ambiente já ativado, já pode instalar os pacotes iniciais principais

```bash
pip install pandas jupyter pip-tools
```

1. Crie um arquivo chamado `requirements.in` e coloque dentro dele o nome dos pacotes instalados

```bash
touch requirements.in
echo $'pandas\njupyter\npip-tools' >> requirements.in
```

1. Para ver o conteúdo do arquivo `requirements.in`, basta executar o comando

```bash
cat requirements.in
```

1. Uma vez com o arquivo `requirements.in` criado e preenchido, vamos gerar o arquivo `requirements.txt` a partir dele utilizando a ferramenta `pip-compile` que foi instalada com o `pip-tools`

```bash
pip-compile requirements.in
```

<aside>
💡 Sempre que instalar um pacote novo, tem que escrever o nome desse pacote em uma nova linha dentro do `requirements.in` e em seguido executar o comando `pip-compile requirements.in` para gerar o arquivo `requirements.txt` atualizado.

</aside>

1. A partir daqui, você deverá ter uma pasta chamada `churn-data-analysis` com os seguintes documentos dentro (o comando abaixo irá mostrar a estrutura descrita):
    1. uma pasta oculta chamada `.venv` (pasta onde os pacotes python desse projeto serão instalados). PS: no linux toda pasta que começa o nome com um `.` é uma pasta oculta
    2. um arquivo chamado `requirements.in` (arquivo de tracking dos pacotes instalados, nesse arquivo você deve escrever o nome dos pacotes que você instalou)
    3. um arquivo chamado `requirements.txt` (arquivo gerado a partir do anterior com as versões específicas para um build determinístico, utilizando o comando `pip-compile` proveniente do pacote `pip-tools`)

```bash
tree -a -L 2
# output
.
├── .venv
│   ├── bin
│   ├── etc
│   ├── include
│   ├── lib
│   ├── pyvenv.cfg
│   └── share
├── requirements.in
└── requirements.txt
```

1. Recomendamos criar mais 3 pastas para:

```bash
mkdir notebooks  # pasta para salvar os códigos notebooks
mkdir data       # pasta para salvar os datasets
mkdir models     # pasta para salvar os modelos treinados
mkdir src        # pasta para salvar pacotes locais ou scripts
```

1. No final, temos a seguinte estrutura inicial de um projeto

```bash
tree -a -L 2
.
├── .venv
│   ├── bin
│   ├── etc
│   ├── include
│   ├── lib
│   ├── pyvenv.cfg
│   └── share
├── data
├── models                
├── notebooks           
├── requirements.in    
├── requirements.txt
└── src                 
```

1. Caso você irá subir esse repositório no github, vale a pena criar um arquivo `.gitignore` que contém instruções para que certos arquivos e pastas não sejam carregados para o github (como as pastas `.venv` e `data`, por exemplo, que não queremos que subam para o github)

```bash
pip install ignr
ignr -p python > .gitignore
echo "data" >> .gitignore
tree -a -L 2

# output
.
├── .gitignore
├── .venv
│   ├── bin
│   ├── etc
│   ├── include
│   ├── lib
│   ├── pyvenv.cfg
│   └── share
├── data
├── models                
├── notebooks           
├── requirements.in    
├── requirements.txt
└── src   
```

1. Para abrir a pasta no VSCode, basta digitar o seguinte comando:

```bash
code .
```

1. **[OPCIONAL]** Caso queira transformar os códigos e scripts dentro da pasta `src` em um pacote python local para ser utilizado dentro dos notebooks, primeiro criamos um arquivo chamado `[setup.py](http://setup.py)` e adicionamos dentro dele o seguinte código:

```python
from setuptools import setup

setup(
    name="src",
    version="0.0.1",
    description="Put your python package description here",
    author="Your name here",
    author_email="Your email here",
		license='MIT',
    url="",
    packages=['src']
)
```

Caso prefira, pode utilizar o arquivo `pyproject.toml` no lugar do `[setup.py](http://setup.py)`. O arquivo pyproject.toml é a forma atual recomendada (veja o [PEP 621](https://peps.python.org/pep-0621/)) pela [Python Package Authority (PyPA)](https://www.pypa.io/en/latest/) para criação de pacotes python. A comunidade python em si ainda não atingiu um consenso a respeito, com muitos grandes projetos ainda utilizando o arquivo `setup.py`. Caso opte pelo `pyproject.toml`, segue abaixo um exemplo mínimo do arquivo:

```python
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "src"
version = "0.0.1"
authors = [
  { name="Example Author", email="author@example.com" },
]
description = "Your package description here"
readme = "README.md"
requires-python = ">=3.7"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]

[project.urls]
"Homepage" = "https://github.com/pypa/sampleproject"
"Bug Tracker" = "https://github.com/pypa/sampleproject/issues"
```

Agora já podemos instalar o pacote localmente, independente se você escolheu o `setup.py`  ou `pyproject.toml`

```bash
pip install -e .
```

`-e` significa que iremos instalar o pacote em modo editável e **`.`** significa que o pacote `src` está no mesmo diretório em que o arquivo `setup.py`

<aside>
⚠️ Toda vez que você adicionar algo novo no pacote, terá que reinstalá-lo com o comando acima e reiniciar o kernel do jupyter notebook em que o pacote está sendo utilizado. Para que você não tenha que fazer isso sempre, ative a seguinte extensão na primeira célula dos jupyter notebooks:

</aside>

```python
%load_ext autoreload
%autoreload 2
```

Com o pacote instalado, imagina que você resolveu escrever uma função chamada `load_data` que é bastante utilizada em várias partes do código, dentro no módulo `data.py` que fica dentro do pacote `src`, como indicado no esquema abaixo:

```bash
.
├── .gitignore
├── .venv
│   ├── bin
│   ├── etc
│   ├── include
│   ├── lib
│   ├── pyvenv.cfg
│   └── share
├── data
├── notebooks
├── requirements.in
├── requirements.txt
└── **src**
    ├── __init__.py
    └── **data.py**
```

Agora podemos, por exemplo, criar um notebook em `notebooks/01_carregando_dados.ipynb` e importar essa função com o seguinte código:

```bash
from src.data import load_data
```