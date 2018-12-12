# Serverless-Django-Application-Build

Included Technologies: CMD, Django, Elastic Beanstalk, Virtualenv

Languages: Python, YAML

This project demonstrates the process to deploy an auto-generated Django website to an Elastic Beanstalk environment while running Python 3.7. The Elastic Beanstalk CLI will be used as the deployment mechanism.

Project walkthrough can be found here: https://www.mikiasemanuel.com/serverless-django-application-build

## Prerequisites:

Python 3.7, pip, virtualenv, and awswebcli must all be installed prior starting the project. 

## Set up Virtual Environment:

### Create virtual environment:
```
virtualenv %HOMEPATH%\ebd-virtual
```

### Activate virtual environment:
``` 
%HOMEPATH%\eb2-virtual\Scripts\activate
```

### Install Django:
```
pip install Django==2.1.1
```

## Develop Django Application: 

### Create Django Project:
```
django-admin startproject ebdjangoproject
```

### Start Django Project:
```
python manage.py runserver
```

## Configuring Django Application for Elastic Beanstalk:

### Define requirements:
```
pip freeze > requirements.txt
```

### Create new directory:
```
mkdir .ebextensions
cd .ebextensions
```

### Add and edit configuration file:
```
type nul > django.config
```
```
option_settings:
 aws:elasticbeanstalk:container:python:
  WSGIPath: ebdjangoproject/wsgi.py
```
```
deactivate
```

## Deploy Django Site with Elastic Beanstalk: 

### Initialize Elastic Beanstalk Environment (eb currently only supports up to python 3.6):
```
eb init -p python-3.6 django-eb-deploy
```

### Set up SSH:
```
eb init
```

### Create and Configure Elastic Beanstalk Environment:

```
eb create ebdjango-project
ebstatus
eb deploy
eb open
```

### Initialize Django’s local database:

```
%HOMEPATH%\ebd-virtual\Scripts\activate’’’
```

### Create Site Administrator:

```
python manage.py migrate
python manage.py createsuperuser
```

### Create Static Assets:

```
Add “STATIC_ROOT = ‘static’” to settings.py
```

```
python manage.py collectstatic
```

```
deactivate
```

### Database Migration Config File:

```
Add configuration file named ‘db-migrate.config’ in /.ebextensions directory
```
```
container_commands:
  01_migrate:
    command: "django-admin.py migrate"
    leader_only: true
option_settings:
  aws:elasticbeanstalk:application:environment:
    DJANGO_SETTINGS_MODULE: ebdjangoproject.settings’’’
```



