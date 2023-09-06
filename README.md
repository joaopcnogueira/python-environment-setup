# Setup de Ambientes Virtuais do Python

# Motivação

Imagine que você ficou responsável por realizar uma análise exploratória sobre dados de churn de uma empresa que solicitou os seus serviços de ciência de dados. Você já recebeu os dados e quer começar a trabalhar com eles, carregando-os no `pandas` e iniciando as análises. Para tal, você precisará realizar o setup de um ambiente python específico para esse projeto, para que não gere conflitos com as dependências e pacotes de outros projetos.

# Pré-Requisitos

1. Sistema Operacional Linux (de preferência Ubuntu), ou
2. Windows Subsystem for Linux (WSL) com Ubuntu instalado, ou
3. Sistema Operacional MacOS, ou
4. ~~Windows~~

> ✅ O Windows pode sim ser utilizado, embora o setup seja um pouco diferente do que é feito em sistemas unix-based (linux e macos). Para mais informações, consultar o google.

5. Verifique se tem o python instalado com versão maior ou igual a 3.7

```bash
python --version
```

6. Tenha um editor de código instalado. Recomendo utilizar o VSCode.

# Passo a Passo

1. Abra o terminal
2. Navegue até a pasta em que você costuma salvar seus projetos utilizando o comando ([como navegar em pastas pelo terminal](https://medium.com/@paulodalves/como-navegar-pelas-pastas-no-terminal-39be96a4c387))
3. Crie a pasta do projeto em que você irá trabalhar digitando o comando abaixo no terminal

```bash
mkdir churn-data-analysis
```

4. Entre na pasta do projeto

```bash
cd churn-data-analysis
```

5. Crie o ambiente virtual do python

```bash
python -m venv .venv
```

6. Ative o ambiente virtual do Python

```bash
source .venv/bin/activate
```

7. Verifique se de fato o ambiente foi ativado solicitando qual python está sendo utilizado com o seguinte comando

```bash
which python
# output: /Users/joaonogueira/churn-data-analysis/.venv/bin/python
```

> ❗ Perceba no output do comando acima que o python que está sendo utilizado é o python que está dentro da pasta `.venv` que por sua vez está dentro da pasta `churn-data-analysis` que foi criada para o projeto.

8. Com o ambiente já ativado, já pode instalar os pacotes iniciais principais

```bash
pip install pandas jupyter pip-tools
```

9. Crie um arquivo chamado `requirements.in` e coloque dentro dele o nome dos pacotes instalados

```bash
touch requirements.in
echo $'pandas\njupyter\npip-tools' >> requirements.in
```

10. Para ver o conteúdo do arquivo `requirements.in`, basta executar o comando

```bash
cat requirements.in
```

11. Uma vez com o arquivo `requirements.in` criado e preenchido, vamos gerar o arquivo `requirements.txt` a partir dele utilizando a ferramenta `pip-compile` que foi instalada com o `pip-tools`

```bash
pip-compile requirements.in
```

> 💡 Sempre que instalar um pacote novo, tem que escrever o nome desse pacote em uma nova linha dentro do `requirements.in` e em seguido executar o comando `pip-compile requirements.in` para gerar o arquivo `requirements.txt` atualizado.

12. A partir daqui, você deverá ter uma pasta chamada `churn-data-analysis` com os seguintes documentos dentro (o comando abaixo irá mostrar a estrutura descrita):
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

13. Recomendamos criar mais 3 pastas para:

```bash
mkdir notebooks  # pasta para salvar os códigos notebooks
mkdir data       # pasta para salvar os datasets
mkdir models     # pasta para salvar os modelos treinados
mkdir src        # pasta para salvar pacotes locais ou scripts
```

14. No final, temos a seguinte estrutura inicial de um projeto

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

15. Caso você irá subir esse repositório no github, vale a pena criar um arquivo `.gitignore` que contém instruções para que certos arquivos e pastas não sejam carregados para o github (como as pastas `.venv` e `data`, por exemplo, que não queremos que subam para o github)

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

16. Para abrir a pasta no VSCode, basta digitar o seguinte comando:

```bash
code .
```

17. **[OPCIONAL]** Caso queira transformar os códigos e scripts dentro da pasta `src` em um pacote python local para ser utilizado dentro dos notebooks, primeiro criamos um arquivo chamado `[setup.py](http://setup.py)` e adicionamos dentro dele o seguinte código:

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

> ⚠️ Toda vez que você adicionar algo novo no pacote, terá que reinstalá-lo com o comando acima e reiniciar o kernel do jupyter notebook em que o pacote está sendo utilizado. Para que você não tenha que fazer isso sempre, ative a seguinte extensão na primeira célula dos jupyter notebooks:

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
└── src
    ├── __init__.py
    └── data.py
```

Agora podemos, por exemplo, criar um notebook em `notebooks/01_carregando_dados.ipynb` e importar essa função com o seguinte código:

```python
from src.data import load_data
```

18. **[OPCIONAL]** Dependências de Desenvolvimento

Uma boa prática é criar dois arquivos de registro de dependências: `requirements.in` e `requirements-dev.in`. No segundo arquivo, anotamos apenas ferramentas e pacotes necessários para a experiência de desenvolvimento, como `jupyter` e o `pip-tools`. Para facilitar o gerenciamento, podemos utilizar um **Makefile**:

```makefile
.PHONY: requirements dependencies

# build requirements.txt from requirements.in only if
# requirements.in is modified
requirements.txt: requirements.in
	pip-compile requirements.in

# build requirements-dev.txt from requirements-dev.in only if
# requirements-dev.in is modified
requirements-dev.txt: requirements-dev.in
	pip-compile requirements-dev.in

# build the requirements.txt and requirements-dev.txt from *.in files,
# only if one of them is modified
requirements: requirements.txt requirements-dev.txt

# install the packagest listed on requirements.txt and requirements-dev.txt
dependencies: requirements
	pip install -r requirements.txt
	pip install -r requirements-dev.txt
```

Copie o conteúdo acima e cole dentro de um arquivo chamado `Makefile`, que deve estar localizado na raiz do projeto. Agora vamos imaginar que uma nova dependência deve ser incluída, como o pacote `scikit-learn`. O passo a passo é:

1. Incluir o nome `scikit-learn` no arquivo `requirements.in`.
2. Atualizar o arquivo `requirements.txt` utilizando o Makefile, através do seguinte comando no terminal:

```makefile
make requirements
```

O mesmo deve ser feito para qualquer outra nova dependência, esteja ela no arquivo `requirements.in` ou `requirements-dev.in`.

Dessa forma, o projeto até fica com os seguintes arquivos:

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
├── Makefile
├── notebooks
├── requirements.in
├── requirements.txt
└── src
    ├── __init__.py
    └── data.py
```
