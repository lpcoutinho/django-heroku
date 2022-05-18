# django-heroku
Configuração mínima para hospedar um projeto Django no Heroku

## Crie o diretório do projeto
- mkdir nomeDaPasta
- cd nomeDaPasta

## Crie e ative uma virtual env

- python3 -m venv nomeDaVenv
- source nomeDaVenv/bin/acttivate

ou

- virtualenv -p python3 .nomeDaVenv
- . .nomeDaVenv/bin/activate

## Instalando o Django
- pip install django

## Crie o projeto django
- django-admin startproject meuProjeto .

O *.* ao fim do código o projeto django será criado direto na raiz do projeto

## Crie o repositório Git
- git init
Crie um arquivo chamado `.gitignore` com o seguinte conteúdo:
```
# Verifique sua IDE
.idea
# Caso estiver usando sqlite3
*.sqlite3
# Nome da virtual env
.vEnv
*pyc
```
- git add .
- git commit -m 'First commit'

## Ocultando a configuração da instância
- pip install python-decouple
- crie o arquivo .env no raiz do projeto e insira as variáveis:
```
SECRET_KEY=suaSecretKey (pegue esta chave em settings.py)
DEBUG=True
```
### Settings.py
- Atualize o código
``` 
from decouple import config

SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
```

## Configure o Banco de Dados
- pip install dj-database-url

### Settings.py
- Atualize o código
```
from dj_database_url import parse as dburl

default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')

DATABASES = { 'default': config('DATABASE_URL', default=default_dburl, cast=dburl), }
```

## Arquivos Static
- pip install dj-static

### wsgi.py
- Atualize o código
```
from dj_static import Cling

application = Cling(get_wsgi_application())
```
- Verifique "DJANGO_SETTINGS_MODULE" 
```
os.environ['DJANGO_SETTINGS_MODULE'] = 'meuSite.settings'
```

### Settings.py
- Atualize o código
``` STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles') ```
## Crie o arquivo requirements-dev.txt
- pip freeze > requirements-dev.txt

## Crie o arquivo requirements.txt adicione uma referência ao anterior e mais dois requerimentos
```
-r requirements-dev.txt
gunicorn
psycopg2
```
## Crie o arquivo Procfile e adicione o seguinte código
```web: gunicorn nomeDoProjeto.wsgi```
- https://devcenter.heroku.com/articles/django-app-configuration

## Crie um arquivo runtime.txt e adicione
- python-3.7.13 (Ou uma versão compatível com o projeto e o heroku)
- https://devcenter.heroku.com/articles/python-support

## Crie o aplicativo no Heroku
Você deve instalar as ferramentas Heroku CLI em seu computador:
https://devcenter.heroku.com/articles/heroku-cli

- heroku apps:create nomeDoApp

## Configurando os hosts permitidos
### settings.py
- Inclua seu url em ALLOWED_HOSTS - Apenas o domínio

## Plugin de configuração de instalação do Heroku
- heroku plugins:install heroku-config

### Enviando configurações de .env para Heroku (você deve estar dentro da pasta onde estão os arquivos .env)
- heroku config:push -a

### Verifique as configurações
- heroku config

## Publique o app
- git add .
- git commit -m 'Configurando app'
- git push heroku master

## Criando o banco de dados (se você estiver usando seu próprio banco de dados não precisa)
- heroku run python3 manage.py migrate

## Criando o usuário admin do Django
- heroku run python3 manage.py createsuperuser
 
## EXTRAS
### Para desabilitar o collectstatic
- heroku config:set DISABLE_COLLECTSTATIC=1

### Alterar uma configuração específica
- heroku config:set DEBUG=True
