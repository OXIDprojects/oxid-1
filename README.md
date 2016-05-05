Oxid Docker
=========

This is an unofficial Docker image for OXID eShop Community Edition.
This images is based on Ubuntu 15.10, Apache2, PHP 5.6 and the current OXID CE version 4.9.8.

## Requirements ##
Use [docker-compose](https://docs.docker.com/compose/) to create your container.

You need a database like MariaDB or MYSQL, with all OXID tables installed or you take the prepared mysql dump
from the demo folder [oxid.sql](demo/db-dumps/oxid.sql).


## Getting started ##

Create a docker-compose.yml with following content:

```yaml
version: '2'
services:
    oxid:
        image: mesa/oxid:latest
        container_name: oxid
        ports:
            - "80:80"
        links:
            - oxid_db:oxid_db
        volumes:
            - "/etc/localtime:/etc/localtime"

        environment:
            MYSQL_HOST: oxid_db
            MYSQL_DATABASE: oxid
            MYSQL_USER: oxid
            MYSQL_PASSWORD: oxid
            OXID_SHOP_URL: "http://localhost"
            OXID_IDEBUG: 0
            OXID_ADMIN_PASSWORD: oxid
            OXID_ADMIN_USERNAME: oxid

    oxid_db:
        image: mariadb:latest
        container_name: oxid_db
#       Open ports for external access in development only
#       ports:
#           - "3306:3306"
        volumes:
            - "./db-dumps/:/docker-entrypoint-initdb.d/"


        environment:
            MYSQL_ROOT_PASSWORD: 25d8341295ed88e4bcfc871970a5bda4
            MYSQL_DATABASE: oxid
            MYSQL_USER: oxid
            MYSQL_PASSWORD: oxid

```
Create a folder named `db-dumps` and put the [demo/db-dumps/oxid.sql](demo/db-dumps/oxid.sql) there


Execute:

```bash
docker-compose up
```

wait and have fun.

Dont forget to change your admin user name and password in your docker-compose.yml, the default values are "oxid".
Only idiots don't change default passwords and you are no idiot aren't you.

```
OXID_ADMIN_PASSWORD: oxid
OXID_ADMIN_USERNAME: oxid
```

It takes up to 20 Seconds until your changed password is changed and written to the database.



##### Backup #####

Change database name, user and password to your config values and your
```bash
docker exec -i oxid_db mysqldump oxid -u oxid -poxid > oxid.sql

docker exec -i oxid_db mysqldump oxid -u oxid -poxid > oxid.sql
```

### Environment Variables ###

Default PHP Variables named like their counterparts in php.ini
```
PHP_ERROR_REPORTING "E_ERROR | E_WARNING | E_PARSE"
PHP_MEMORY_LIMIT "256M"
PHP_DATE_TIMEZONE "Europe/Berlin"
PHP_DISPLAY_ERRORS "Off"
PHP_UPLOAD_MAX_FILESIZE  "8m"
```


This docker v-host configuration variables
```
DOCKER_DOCUMENT_ROOT "/data/"
DOCKER_SERVER_ADMIN "admin@localhost"
DOCKER_ERROR_LOG "/dev/stdout"
DOCKER_CUSTOM_LOG "/dev/stdout combined"
DOCKER_ALLOW_OVERRIDE "All"
```

Apache2 default configuration values
```
APACHE_RUN_USER "www-data"
APACHE_RUN_GROUP "www-data"
APACHE_LOG_DIR "/dev/stdout"
APACHE_LOCK_DIR "/var/lock/apache"
APACHE_PID_FILE "/tmp/apache2.pid"
APACHE_SERVERNAME "localhost"
```

Oxid admin user credentials
```
OXID_ADMIN_PASSWORD "docker"
OXID_ADMIN_USERNAME "docker"
```

OXID configuration from  config.inc.php
```
OXID_SHOP_URL "http://localhost"
OXID_UTF_MODE 1
OXID_IDEBUG 0
OXID_COMPILE_DIR "/tmp"
```

OXID database configuration, equal names for mariaDB/mysql container variables.
You have to define them for both webserver image and database image
```
MYSQL_HOST "oxid_db"
MYSQL_USER "oxid"
MYSQL_PASSWORD "oxid"
MYSQL_DATABASE "oxid"

```


### Versions: ###
* OXID eShop Community Edition. 4.9.8
* Apache/2.4.12 (Ubuntu)
* Ubuntu 15.10



## [Docker MariaDB](https://hub.docker.com/_/mariadb/) ##
Put your database sql dump inside the folder `db-dumps` and on every rebuild of
your database container the sql inside


## License
The MIT License (MIT)