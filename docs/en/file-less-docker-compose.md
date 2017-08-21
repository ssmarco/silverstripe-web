## File-less docker-compose (Multi-server configuration)

This is a guide in how silverstripe-web can be used in conjunction with tools such as Composer to create environments without having to define a `docker-compose.yml` or manually create an nginx-proxy / networks.

### Introduction

As a developer who develops / maintains a large number of SilverStripe websites I was getting tired of having to create and re-create the same `docker-compose.yml` file for each project / environment and having to type long commands for what I found was crucial for my development. So I begun to think of solutions to this problem.

As I develop on multiple environments (Windows & Mac), I wanted a solution that could work for multiple operating systems as well as being easy to install. Composer was the tool I chose to handle these problems as it is already multi-OS compatible and is already used heavily with SilverStripe applications.

[brettt89/ss-docker-compose](https://github.com/brettt89/ss-docker-compose) is a composer project which (in a nut shell) installs a binary which wraps the `docker-compose` command to provide additional functionality.

### Installation Instructions

To install run:
`composer global require brettt89/ss4-docker-compose`

Make sure your `~/.composer/vendor/bin` directory is in your PATH.
`echo 'export PATH=$PATH:~/.composer/vendor/bin/'  >> ~/.bash_profile`

### How it works

ss-docker-compose installs the `ss-docker` binary and a couple of `docker-compose.yml` to your local composer directory. These tools hold most of the necessary components to start a silverstripe-web environment with a database and nginx-proxy

#### ss-docker

`ss-docker` wraps `docker-compose` to define specific configuration files when commands are run, it also ensures that when you run `docker-ss up -d` that nginx-proxy is running and networks are all connected. It is intended to be used instead of `docker-compose`.

E.g.

```console
## Create nginx-proxy, web server, database and links them all together.
$ docker-ss up -d

## Stops web server and database
$ docker-ss stop

## Start web server and database
$ docker-ss start

## Run dev/build on SilverStripe application
$ docker-ss exec web php framework/cli-script dev/build
```

#### Environment file

This setup requires only an environment file defining the connection for the database. Server is identified by the domain "database" and user "root" has no password.

E.g. \_ss\_environment.php (SilverStripe 3.x)

```php
...
define('SS_DATABASE_SERVER', "database");
define('SS_DATABASE_USERNAME', "root");
define('SS_DATABASE_PASSWORD', "");
define('SS_DATABASE_NAME', "SS_mysite");
...
```

.env (SilverStripe 4.x)

```env
SS_DATABASE_CLASS = "MySQLPDODatabase"
SS_DATABASE_SERVER = "database"
SS_DATABASE_USERNAME = "root"
SS_DATABASE_PASSWORD = ""
SS_DATABASE_NAME = "SS_mysite"
```

#### Domains / Project Directory

By default brettt89/ss-docker-compose uses the "Project Directory" as the Domain and Name for each project.

E.g. Project location: ~/src/sites/project1
Web Server: http://project1.local/
Database Server: project1.db.local

NOTE: All domains will end with .local by default.

`ss-docker` should always be used in the root directory of your project. This is because it mounts your currently directory to the web server container.

#### SSH / bash

Normally when you need to SSH into a docker environment, you need to execute `/bin/bash` on the container by using a command similar to `docker-compose exec <container> /bin/bash`.

To eliminate this long command we have added a custom command `ss-docker ssh` which will open a bash terminal on the web server in your project's directory.

```console
$ ss-docker ssh
$ root@665b4a1e17b6:/var/www/html#
```

### How to use

Create a new SilverStripe project and change directory into project.

```console
$ composer create-project silverstripe/installer ~/src/sites/project1

$ cd ~/src/sites/project1
```

Start environment

```console
$ ss-docker up -d
```

Your Environment should now be ready. As you ran ss-docker in the folder "project1". This means your Environment will also use this folder name as your domain.

E.g. http://project1.local/

You can also ssh into the project so you can run commands via CLI.
(E.g. running dev/build)

```console
$ ss-docker ssh
$ root@665b4a1e17b6:/var/www/html#
$ root@665b4a1e17b6:/var/www/html# php framework/cli-script.php dev/build
```
