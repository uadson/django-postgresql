# django-postgresql
Integração Django e SGBD PostgreSQL

## Preparando o Ambiente

1. Criando um ambiente virtual para separar este projeto dos demais, bem como reservar suas dependências.

    python -m venv nome_do_seu_virtual_enviroment

    ex.: python -m venv myproject_env

2. Após criado o ambiente virtual, é preciso ativá-lo para prosseguir com a instalação dos pacotes.

    2.1 Ativando o virtual enviroment:

        Exemplos:

            windows

            C:\pasta_do_projeto\myproject_env\Scripts\activate


            linux

            /home/user/pasta_do_projeto/source myproject_env/bin/activate

    2.2 Ao ser ativado o nome que foi dado ao ambiente virtual ficará entre parenteses antes do path do projeto:

        ex.:

        (myproject_env) C:\pasta_do_projeto\


3. Com o ambiente virtual ativado é hora de instalar o Django e suas dependências:

        pip install django

4. Instalando o adaptador para conectarmos ao PostgreSQL:

        pip install psycopg2

5. Instalando um pacote para podermos ocultar os dados importantes do projeto como 
   secret key, acesso ao bando de dados, etc.

        pip install python-decouple

6. Criando um arquivo de texto, no qual conterá todas as dependências do projeto.

        pip freeze > requirements.txt

## Construindo o Projeto/Aplicação

Obs.: todos os comandos de agora em diante dependem de o ambiente virtual estar ativado,
      caso contrário, retornará uma mensagem de erro.

1. Criando o projeto django.

    django-admin startproject myproject .


    1.1 Para saber se está tudo instalado corretamente digite o comando:

        python manage.py runserver

    No browser do navegador digite 127:0.0.1:8000 e pressione enter

    Se retornar uma tela como a tela abaixo, está tudo ok com o projeto, caso contrário,
    reveja os passos anteriores.

![tela_inicial_django](https://user-images.githubusercontent.com/62815552/120864079-079b0980-c562-11eb-883d-cdee9ffa3981.png)


2. Criando uma aplicação.

Quando o projeto foi criado, um estrutura de arquivos e diretórios foi criada.

Um desses arquivos é o manage.py. Ele foi usado no tópico anterior para rodar o servidor.

Para criar o projeto, basta digitar o comando:

    python manage.py startapp myapp

Após definição do projeto e aplicação, algumas configurações básicas são necessárias:


## Configurações

1. Adicionando o app (myapp) ao projeto (myproject).

No Framework django, é possível criar várias aplicações em um único projeto. Da mesma maneira que se tem o myapp vinculado ao projeto myproject, poderia ser ter myapp1, myapp2, myapp3 ..., todos vinculados ao mesmo projeto myproject.


As configurações serão efetuadas no arquivo settings.py que está no diretório myproject.

Dentro dele há o código abaixo:

        # Application definition

        INSTALLED_APPS = [
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
        ]

O app (myapp) será incluído ao projeto através da lista INSTALLED_APPS:

        # Application definition

        INSTALLED_APPS = [
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
            'myapp',
        ]

2. Ocultando os dados de autenticação do banco de dados e de outras variáveis.
Python-Decouple é uma excelente biblioteca para proteger tais parâmetros.

Deve se criar um arquivo .env no diretório raiz do projeto, e nele devem ser informados os parâmetros que se deseja ocultar ao realizar o commit por exemplo, e tais dados não subirem para o repositório no nuvem. Para isso o arquivo .env deve ser informado no arquivo .gitignore.

        
        # Environments
        .env
        

2.1 Após criação do arquivo .env, deve-se incluir parâmetros, os quais se deseja ocultar ao commitar o projeto.

Neste projeto os dados protegidos serão:

        SECRET_KEY = 'Be>31d>1jB84'?Yl`LMQS\:(~o':QK[j>@nFly(H:`H)M!4Un!KOsf>d_}xLMHa.u'

        DEBUG = True
        
        ALLOWED_HOSTS = []
        
        DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'db_django_psql',
            'USER': 'admin',
            'PASSWORD': 'H)M!4Un!KOsf',
            'HOST': 127.0.0.1,
            'PORT': ''
        }
    }

Estes dados serão informados no arquivo .env (sem as aspas):

        SECRET_KEY = Be>31d>1jB84'?Yl`LMQS\:(~o':QK[j>@nFly(H:`H)M!4Un!KOsf>d_}xLMHa.u
        DEBUG = True
        ALLOWED_HOSTS = 127.0.0.1
        DB_NAME = db_django_psql
        DB_USER = admin
        DB_PASSWORD = H)M!4Un!KOsf 
        DB_HOST = 127.0.0.1


Assim feito, no arquivo settings.py os dados agora ficarão da seguinte forma:

    Importando a biblioteca e funções

        from decouple import config, Csv


            SECRET_KEY = config('SECRET_KEY')

            DEBUG = config('DEBUG', cast=bool)
            
            ALLOWED_HOSTS = config('ALLOWED_HOSTS', cast=Csv())
            
            DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.postgresql_psycopg2',
                'NAME': config('DB_NAME'),
                'USER': config('DB_USER'),
                'PASSWORD': config('DB_PASSWORD'),
                'HOST': config('DB_HOST'),
                'PORT': ''
            }
        }
