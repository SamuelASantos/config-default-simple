# Django Configuração de Projeto Padrão

Requisitos:
- Python instalado na máquina

Criar o ambiente virtual Linux/Windows
## Ambiente Virtual Linux/Windows
### Windows
- python -m venv .venv
- source .venv/Scripts/activate # Ativar ambiente

### Linux 
##### Caso não tenha virtualenv. "pip install virtualenv"
- virtualenv .venv
- source .venv/bin/activate # Ativar ambiente
---------------------------------------------------------

Instalar os seguintes pacotes:
- pip install django
- pip install pillow

Para criar o arquivo requirements.txt
- pip freeze > requirements.txt

Para instalar na máquina o arquivo requirements.txt basta digitar:
- pip install -r requirements.txt

---------------------------

## Criando o Projeto
“myProject” é nome do seu projeto e quando colocamos um “.” depois do nome do projeto significa que é para criar os arquivos na raiz da pasta. Assim não cria subpasta do projeto.
- django-admin startproject myProject .

Testar a aplicação
- python manage.py runserver

---------------------
## Configurar Settings e Arquivos Static
```python
import os 

# base_dir config
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
TEMPLATE_DIR = os.path.join(BASE_DIR,'templates')
STATIC_DIR=os.path.join(BASE_DIR,'static')

# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'), 
    }
}

STATIC_ROOT = os.path.join(BASE_DIR,'static')
STATIC_URL = '/static/' 

MEDIA_ROOT=os.path.join(BASE_DIR,'media')
MEDIA_URL = '/media/'

# Internationalization
# Se quiser deixar em PT BR
LANGUAGE_CODE = 'pt-br'
TIME_ZONE = 'America/Sao_Paulo'
USE_I18N = True
USE_L10N = True 
USE_TZ = True
```
myProject/urls.py
```Python
from django.contrib import admin
from django.conf import settings
from django.conf.urls.static import static
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # Adicionar Isto
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Adicionar Isto
```
----------------------
## Criando Aplicativo
Para criar a aplicação no Django rode comando abaixo. “myapp” é nome do seu App.
- python manage.py startapp myapp

Agora precisamos registrar nossa aplicação no INSTALLED_APPS localizado em settings.py.

---------------------------------

## Template base e Bootstrap Configuração
Com Base na documentação para utilizar os recursos Boostrap basta adicionar as tags de CSS e JS. No HTML da Pagina Base.

[Bootstrap5 - Documentação](https://getbootstrap.com/docs/5.2/getting-started/introduction/)

---------------------------
### Template Base
1- Criar uma pasta 'templates' dentro de myapp

2- Criar um arquivo base base.html onde vamos renderizar nosso conteúdo.

```Python
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>{% block title %}{% endblock %}</title>

	<!-- CSS -->
	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">

</head>
<body>  
	
	{% block content %}
	
	{% endblock %} 
 
	<!-- JS-->
	<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
</body>
</html>
```

---------------
### Criar uma View
1- Na pasta templates criar um novo arquivo index.html

index.html
```Python
{% extends 'base.html' %}
{% block title %}Pagina Inicial{% endblock %}
{% block content %}
	<h1>Pagina Inicial</h1>
{% endblock %}
```
myapp/views.py
```Python
from django.shortcuts import render

# Create your views here.
def mysite(request):
    return render(request, 'index.html')
```

1- criar arquivo myapp/urls.py

2- Adicionar o código abaixo

```Python
from django.urls import path 
from myapp import views

urlpatterns = [
    path('', views.mysite, name='mysite'), 
]
```
urls.py do projeto. myproject/urls.py
```Python
from django.contrib import admin
from django.urls import path, include # adicionar include
from django.conf import settings
from django.conf.urls.static import static 

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')), # url do app
]
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # Adicionar Isto
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Adicionar Isto
```
Rodar o projeto para ver como está
```Python
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```
.gitignore
```git
/tmp
passenger_wsgi.py 
.venv
db.sqlite3
/static_media
static_media
/static_files
static_files
/media
mydatabase
file_name.sql
__pycache__ 
__pycache__/
```