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

## Construindo o Projeto

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