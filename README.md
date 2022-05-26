# django-heroku
Configuração mínima para hospedar um projeto Django no Heroku
- Baseado em: https://www.youtube.com/watch?v=KczDMzeyiLU

## Crie o diretório do projeto
- mkdir nomeDaPasta
- cd nomeDaPasta

## Crie e ative uma virtual env

- python3 -m venv venv
- source venv/bin/acttivate

## Instalando o Django
- pip install django

## Crie o projeto django
- django-admin startproject meuProjeto .

O *.* ao fim do código o projeto django será criado direto na raiz do projeto

## Crie o repositório Git
Crie um arquivo chamado `.gitignore`
https://github.com/github/gitignore/blob/main/Python.gitignore

- git init
- git add .
- git commit -m 'First commit'

## Ocultando a configuração da instância
- pip install python-decouple
- crie o arquivo .env no raiz do projeto e insira as variáveis:
```
SECRET_KEY=secret
DATABASE_URL=sqlite:///db.sqlite3
DEBUG=True
```
### Settings.py
- Atualize o código
``` 
from decouple import config

SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
```
## Configurando os hosts permitidos
### settings.py
- Inclua seu url em ALLOWED_HOSTS - Apenas o domínio
ou
`ALLOWED_HOSTS = ['*']
`
## Configure o Banco de Dados
- pip install dj-database-url

### Settings.py
- Atualize o código
```
import dj_database_url

DATABASES = {}

DATABASES['default'] = dj_database_url.config(conn_max_age=600, ssl_require=True)
DATABASES['default'] = dj_database_url.config(default=config('DATABASE_URL'))
```

## Arquivos Static
- pip install whitenoise

### Settings.py
- Atualize o código
``` 
MIDDLEWARE = [
    # ...
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",
    # ...
]

STATIC_ROOT = BASE_DIR / 'static/'
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
```
## Faça uma migração
- python manage.py migrate

## Instale o adaptador de BD
- pip install psycopg2

## Crie um arquivo runtime.txt e adicione sua versão do python
- python-3.7.13 (Ou uma versão compatível com o projeto e o heroku)
- https://devcenter.heroku.com/articles/python-support

## Instale o gunicorn
- pip install gunicorn

## Crie o arquivo requirements.txt
- pip freeze > requirements.txt

## Crie o arquivo Procfile e adicione o seguinte código
```web: gunicorn nomeDoProjeto.wsgi --log-file -```
- https://devcenter.heroku.com/articles/django-app-configuration

## Atualize o repositório Git
- git add .
- git commit -m 'congigurando app'
- git branch -M main
- git remote add origin seuRepositórioGit
- git push -u origin main

-------------------- testar --------------------
## Crie o aplicativo no Heroku
Você deve instalar as ferramentas Heroku CLI em seu computador:
https://devcenter.heroku.com/articles/heroku-cli

- heroku apps:create nomeDoApp

## Plugin de configuração de instalação do Heroku
- heroku plugins:install heroku-config

### Enviando configurações de .env para Heroku (você deve estar dentro da pasta onde estão os arquivos .env)
- heroku config:push -a

### Verifique as configurações
- heroku config

## Publique o app
- git add .
- git commit -m 'Configurando app'
- git push heroku main

## Criando o banco de dados (se você estiver usando seu próprio banco de dados não precisa)
- heroku run python3 manage.py migrate

## Criando o usuário admin do Django
- heroku run python3 manage.py createsuperuser
 
## EXTRAS
### Para desabilitar o collectstatic
- heroku config:set DISABLE_COLLECTSTATIC=1

### Alterar uma configuração específica
- heroku config:set DEBUG=True


-------------------- depois de arquivos staticos -----------------
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

## Plugin de configuração de instalação do Heroku
- heroku plugins:install heroku-config

### Enviando configurações de .env para Heroku (você deve estar dentro da pasta onde estão os arquivos .env)
- heroku config:push -a

### Verifique as configurações
- heroku config

## Publique o app
- git add .
- git commit -m 'Configurando app'
- git push heroku main

## Criando o banco de dados (se você estiver usando seu próprio banco de dados não precisa)
- heroku run python3 manage.py migrate

## Criando o usuário admin do Django
- heroku run python3 manage.py createsuperuser
 
## EXTRAS
### Para desabilitar o collectstatic
- heroku config:set DISABLE_COLLECTSTATIC=1

### Alterar uma configuração específica
- heroku config:set DEBUG=True
