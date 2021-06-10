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
Python-Decouple é uma excelente biblioteca para proteger tais parâmetros. Saiba mais sobre a biblioteca aqui: https://pypi.org/project/python-decouple/

Deve se criar um arquivo .env ou .ini no diretório raiz do projeto, e nele devem ser informados os parâmetros que se deseja ocultar ao realizar o commit por exemplo, e tais dados não subirem para o repositório no nuvem. Para isso o arquivo .env deve ser informado no arquivo .gitignore.

        
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

## Banco de Dados

1. Por padrão Django possui alguns Apps implementados, conforme a lista INSTALLED_APPS no arquivo settings.py.

O função migrate fará com que as tabelas relacionadas as estes Apps, sejá implementadas no banco de dados.


        python manage.py migrate


Resultado:


        Apply all migrations: admin, auth, contenttypes, sessions
        Running migrations:
          Applying contenttypes.0001_initial... OK
          Applying auth.0001_initial... OK
          Applying admin.0001_initial... OK
          Applying admin.0002_logentry_remove_auto_add... OK
          Applying admin.0003_logentry_add_action_flag_choices... OK
          Applying contenttypes.0002_remove_content_type_name... OK
          Applying auth.0002_alter_permission_name_max_length... OK
          Applying auth.0003_alter_user_email_max_length... OK
          Applying auth.0004_alter_user_username_opts... OK
          Applying auth.0005_alter_user_last_login_null... OK
          Applying auth.0006_require_contenttypes_0002... OK
          Applying auth.0007_alter_validators_add_error_messages... OK
          Applying auth.0008_alter_user_username_max_length... OK
          Applying auth.0009_alter_user_last_name_max_length... OK
          Applying auth.0010_alter_group_name_max_length... OK
          Applying auth.0011_update_proxy_permissions... OK
          Applying auth.0012_alter_user_first_name_max_length... OK
          Applying sessions.0001_initial... OK

Ao utilizar o gerenciador de banco de dados PgAdmin por exemplo, é possível visualizar as tabelas criadas no banco.

![pgadmin](https://user-images.githubusercontent.com/62815552/121387194-e1d68180-c920-11eb-9a00-7cdb19047a92.png)


## Models

1. O meio pelo qual se pode popular o banco com os dados relacionados a aplicação, é através do arquivo models.py.
Nele serão criados os campos para captação dos dados e posteriormente, a inserção dos mesmos ao banco de dados.


Exemplo:

    1        from django.db import models
    2
    3        # Create your models here.
    4        class Pessoa(models.Model):
    5            nome = models.CharField(max_length=200, blank=False)
    6
    7            class Meta:
    8                db_table = 'pessoa'
    9
    10            def __str__(self):
    11                return self.nome

O exemplo acima representa a criação de uma tabela e uma coluna cujo o atributo é 'nome':

Linhas:

1 - importação do módulo models

4 - criação da classe Pessoa herdando de Models os parâmetros para serem aplicados à classe, e também será o nome da tabela

5 - nome = é o atributo da classe Pessoa e que será portanto o atributo da coluna da tabela.

7 - quando se cria uma classe, o seu nome será o nome da tabela, e consequentemente, para realizar a relação da aplicação e suas tabelas, por padrão, o nome da tabela ao ser criada será, no caso do projeto em questão, myapp_pessoa; para que o nome da tabela não tenha o nome da aplicação inserido nele, utiliza-se a class Meta com o atributo db_table; o nome da tabela ficará então como 'pessoa';

10 - o método __str__(self), considerado como um dos métodos mágicos em Python, retornará o nome do atributo nome,
como uma string e não como o nome genérico do objeto.

## Makemigrations

Após criada a classe é hora de implementar essa tabela no projeto.
Para isso é utilizado o método makemigrations

Após toda alteração realizada no arquivo models.py é necessário realizar a execução deste comando para que as
alterações sejam efetivadas.

        python manage.py makemigrations

Resultado:

        Migrations for 'myapp':
        myapp/migrations/0001_initial.py
            - Create model Pessoa

*Importante: como o projeto possui uma aplicação apenas - myapp - não há problema em realizar a execução do 
comando makemigrations sem parâmetros. Mas a partir do momento em que houver mais de uma aplicação, é 
importante que se informe a aplicação na qual será efetuadas as alterações.

Ex.:

        python manage.py makemigrations myapp2

## Migrate

Logo após a execução do comando makemigrations, para que a tabela seja criada no banco de dados, é preciso 
executar o comando migrate.

Um método interessante de se executar antes do migrate é sqlmigrate.

Ele mostra previamente como a tabela será criada no banco de dados.

Para isso é preciso executar o método, o nome da aplicação e número o código que foi gerado quando executado 
o método makemigrations.

Ex.: 

        python manage.py sqlmigrate myapp 0001

Resultado:

        BEGIN;
        --
        -- Create model Pessoa
        --
        CREATE TABLE "pessoa" ("id" bigserial NOT NULL PRIMARY KEY, "nome" varchar(200) NOT NULL);
        COMMIT;

Em seguida o método migrate para implementar a tabela no banco de dados


        python manage.py migrate

        ou

        python manage.py migrate myapp

Resultado:

        Operations to perform:
        Apply all migrations: myapp
        Running migrations:
        Applying myapp.0001_initial... OK


E agora pode-se ver a tabela criada com o nome 'pessoa' no banco de dados:

![pg4](https://user-images.githubusercontent.com/62815552/121532721-ffb1ee00-c9d5-11eb-9cab-d51587a24b23.png)
