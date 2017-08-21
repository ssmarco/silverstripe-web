# Docker-compose Development Environment

This guide will help you set up a docker-compose development environment for your SilverStripe project.

## Docker Compose Configuration

Compose is a tool for defining and running multi-container Docker applications. To learn more about Compose refer to the [Compose Documentation](https://docs.docker.com/compose/overview/)

First we will need to create a `docker-compose.yml` file to define our environment. This should contain our `silverstripe-web:5.6-platform-dev` web server and a database. In this example we will use [MySQL](https://hub.docker.com/r/_/mysql/).

### Create a `docker-compose.yml` in your SilverStripe project

```yml
version: '3'
services:
  # Web server
  web:
    image: brettt89/silverstripe-web:5.6-platform-dev
    working_dir: /var/www/html
    volumes:
      - .:/var/www/html

  # Database
  database:
    image: mysql
    volumes:
      - db-data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ALLOW_EMPTY_PASSWORD=true

# We define an external volume for the database to persist its data between start / stops
volumes:
  db-data:
```

Like all SilverStripe applications, we will need an environment file to define some default SilverStripe variables and connection details for the Database.

### Create an `_ss_environment.php` in your SilverStripe project

You would also use this file to define custom environment variables used in your SilverStripe application.

```php
<?php

/* Change this from 'dev' to 'live' for a production environment. */
define('SS_ENVIRONMENT_TYPE', 'dev');

/* This defines a default database user */
define('SS_DATABASE_SERVER', 'database');
define('SS_DATABASE_NAME', 'SS_mysite');
define('SS_DATABASE_USERNAME', 'root');
define('SS_DATABASE_PASSWORD', '');

/* Configure a default username and password to access the CMS on all sites in this environment. */
define('SS_DEFAULT_ADMIN_USERNAME', 'admin');
define('SS_DEFAULT_ADMIN_PASSWORD', 'password');

// This is used by sake to know which directory points to which URL
global $_FILE_TO_URL_MAPPING;
$_FILE_TO_URL_MAPPING['/var/www/html'] = 'http://localhost';
```

Now we have a basic setup configured, now we just start the environment.

### Start environment using `docker-compose`

```console
$ docker-compose up -d
```

Congratulations, you now have a running docker environment with SilverStripe installed. You should now be able to access your SilverStripe environment via [http://localhost/](http://localhost/) or the IP of the container.

#### Additional documentation you may find useful.

 - [Using nginx-proxy for domains](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/nginx-proxy-domain.md)
 - ["File-less" docker-compose](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/file-less-docker-compose.md)
