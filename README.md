[![Build Status](https://travis-ci.org/xchem/fragalysis-backend.svg?branch=master)](https://travis-ci.org/xchem/fragalysis-backend)
[![stable](http://badges.github.io/stability-badges/dist/stable.svg)](http://github.com/badges/stability-badges)
[![Version](http://img.shields.io/badge/version-0.0.1-blue.svg?style=flat)](https://github.com/xchem/fragalysis-backend)
[![License](http://img.shields.io/badge/license-Apache%202.0-blue.svg?style=flat)](https://github.com/xchem/fragalysis-backend/blob/master/LICENSE.txt)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/xchem/fragalysis-backend.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/xchem/fragalysis-backend/context:python)

# Fragalysis backend
Django server for Fragalysis with DRF API and loaders for data. Has components to serve React web-app but doesn't actually do that.

*NOTE:* Ensure `DEBUG=False` when using this in production but for development you can switch this on to receive thorough error messages. 

# Database and Migrations

An issue that we encountered was that the Django migrations failed to apply when recreating starting the fragalysis-frontend, in order to resolve this:

    1. Start up the stack via the fragalysis-frontend: `docker-compose -f docker-compose.localhost.yml`

    2. Attach a shell to the fragalysis-stack docker container ("web_dock").

    3. In the stack shell delete all present migrations with:

        - `find . -path "*/migrations/*.py" -not -name "__init__.py" -delete`
        - `find . -path "*/migrations/*.pyc"  -delete`

    4. Keep this shell running.

    5. Attach a new shell (/bin/bash) to the running mysql docker container ("fragalysis-frontend_mysql").

    3. Once you have a new shell to the database, you can login and drop the "django_db" database schema and recreate it:

        - Login with `mysql -u root -p` and when prompted for a password, enter `password`. 
        
        - Run the following SQL command to delete the schema: `DROP DATABASE django_db;`

        - Run the following SQL command to create a fresh schema: `CREATE DATABASE django_db;`

    4. The "django_db" database should now have no tables, you can close the docker container.

    5. In the fragalysis-stack shell, you can ensure the migrations will apply correctly by create the migrations again and running them:

        * Make new migrations: `python manage.py makemigrations`

        * Apply migrations: `python manage.py migrate` 

