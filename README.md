#May 29, 2018, Tues 
section 2, lecture 12 beg. 


#Django2.0.5
source ~/django2/bin/activate

#Super User
username: hal
email: halvong@yahoo.com
password: wesleyave

#code original
example folder
git chapter [#,17]
git next
git prev

#creates super user
docker-compose exec web python manage.py createsuperuser

#creates a shell
1. docker run -i -t --rm -v $PWD:/usr/src/app python:3.6.5 bash
2. pip install --upgrade pip
3. pip install Django==2.0.5
4. django-admin startproject mysite
6. exit; sudo chown -R $USER:$USER mysite
7. source ~/venv3/bin/activate
8. cd [my project]; python manage.py startapp [app name]
9. add classes to models.py for each apps
10. python manage.py makemigrations [app name]  
11. docker build . 
12. move/create Dockerfile, docker-compose.yml, README.md, requirements.txt to root directory
13. docker run -p 8000:8000 [image id] \
    python manage.py runserver 0.0.0.0:8000
14. change settings.py for time_zone, databases, use_tz
15. docker tag [image id] [image name:1.0]
16. docker run -p 8000:8000 [image name]
17. docker-compose up --build
18. docker-compose up -d database 
19. docker-compose run --rm database psql -U postgres -h database; 
    create database [name]
20. docker-compose build web 
21. docker-compose up -d web 
22. docker-compose exec web python manage.py makemigrations books
23. docker-compose exec web python manage.py sqlmigrate books 0001 
24. docker-compose exec web python manage.py migrate
25. docker-compose exec web python manage.py createsuperuser
25. docker-compose exec web python manage.py shell

#starts individually
0. docker-compose up --build
1. docker-compose up -d database 
2. docker-compose logs database 
3. docker-compose build web 
4. docker-compose up -d web 
2. docker-compose logs -f web 
5. docker-compose exec web python manage.py makemigrations books
6. docker-compose exec web python manage.py sqlmigrate books 0001 
7. docker-compose exec web python manage.py migrate
8. docker-compose run --rm database psql -U postgres -h database
   password: postgres

#django_site does not exists error
./manage.py migrate sites
./manage.py migrate

#check containers running
docker-compose ps

#starts all containers
docker-compose up -d 

#----
#gunicorn manual
gunicorn --bind 0.0.0.0:8000 wisdompets.wsgi:application

#----
#migrate
python manage.py makemigrations books
python manage.py sqlmigrate blog 0001
#assume django in container 
docker-compose exec [web] python manage.py sqlmigrate blog 0001
docker-compose exec [service] python manage.py migrate
docker-compose exec web python manage.py collectstatic

#force stop
docker stop $(docker ps -a -q)
#remove
docker rm $(docker ps -a -q)

#--------------------------------
#uses Dockerfile
#remove all containers
docker rm $(docker ps -a -q)

#remove all images
docker rmi $(docker images -q)

#--------------------------------
#image
#run an image
docker run -p 8000:8000 8c3ff7015517 \
> python3 manage.py runserver 0.0.0.0:8000

#rebuild existing image using tag
docker build -t [app_name] .

#run an image
docker run -p 8000:8000 [app_name]
#--------------------------------
#containers, login to container
docker exec -t -i [66175bfd6ae6] bash

#--------------------------------
#Docker-compose

#build image and container
docker-compose up --build

#run container and detached
docker-compose up -d

#service
docker-compose [start,stop,restart] [service]

#--------------------------------
#database
#connecting to database container
docker-compose up -d database
docker-compose logs database
docker-compose run --rm database psql -U postgres -h database

#app.settings database config
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'django_core',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'database',
        'PORT': 5432,
    }
}
#--------------------------------
#database log
docker-compose logs database

#--------------------------------
#web
#database web
docker-compose logs web

#--------------------------------
#virtualenv
source ~/venv3/bin/activate

#django version
python -m django --version
python manage.py startapp books
python manage.py runserver

#--------------------------------
#postgresql
\l: list all databases
\dt: list all tables in current database
\c: connects to database
\d+ <tablename>
\x: expanded display
#--------------------------------
#git
from django.conf import settings
dir(settings)
settings.name