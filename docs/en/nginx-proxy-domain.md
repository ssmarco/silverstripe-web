## Nginx-proxy for domains

When working with development environment/s it is common practice to use a specific domain to access your environment rather than localhost or IP address. Nginx-proxy takes the hassle out of having to manually check your docker container for its IP and setup your hosts file every time you create a new environment or your IP changes.

With nginx-proxy, each time we start a silverstripe-web container, nginx-proxy listens to docker and looks for the environment variable VIRTUAL_HOST. If found, nginx-proxy will create a forwarding rule to point the domain defined in VIRTUAL_HOST to the container that was just created.

E.g. VIRTUAL_HOST=silverstripe.dev
http://silverstripe.dev => nginx-proxy => silverstripe-web container.

This can be especially useful when running multiple development environments as you can use tools such as `dnsmasq` to point \*.dev/ to nginx-proxy. This eliminates the need for adding entries for each domain you want to use.

> NOTE: A clone of jwilder/nginx-proxy is used [https://github.com/brettt89/nginx-proxy](https://github.com/brettt89/nginx-proxy) for additional configuration options

### Create a `docker network` for your projects

Before we create our proxy, we will need to define a network in which all our containers will start in. This is so the nginx-proxy can access your SilverStripe environment/s simultaneously.

```console
$ docker network create my-network
```

### Run `nginx-proxy` in your network

Nginx proxy will act as a gateway for all the project environments. For best results it is recommended to use a shared root domain for each project. E.g. `dev`

```console
$ docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro --net my-network --name nginx-proxy brettt89/nginx-proxy
```

### Add my-network and VIRTUAL_HOST to your `docker-compose.yml` file/s

The final step is to add the VIRTUAL_HOST environment to the silverstripe-web Docker image and add the network we created at the beginning of this guide.

In this example we are using the domain http://project1.dev/.

```yml
version: '3'
services:
  web:
    image: brettt89/silverstripe-web:5.6-platform-dev
    working_dir: /var/www
    volumes:
      - .:/var/www/html
    # Add VIRTUAL_HOST environment variable for web server
    environment:
      - VIRTUAL_HOST=project1.dev

  database:
    image: mysql
    volumes:
      - db-data:/var/lib/mysql
    restart: always
    # Add VIRTUAL_HOST environment variable for database
    environment:
      - MYSQL_ALLOW_EMPTY_PASSWORD=true
      - VIRTUAL_HOST=project1.db.dev

volumes:
  db-data:

# Add network definition for nginx-proxy
networks:
  default:
    external:
      name: my-network
```

#### Start / Restart your environment

```console
$ docker-compose up -d
```

### Point your domain at nginx-proxy

Now that we have nginx-proxy running in our network. This will be our single source in which we can point ALL our domains at for containers running in the same network as nginx-proxy.

Mac OSX / Linux:
Use `127.0.0.1` (localhost)

Windows:
Check your Docker application for Host IP.

```/etc/hosts
127.0.0.1    project1.dev project1.db.dev
```

### Access your domain

Now you should be able to access your environment via the domain provided in the VIRTUAL_HOST environment variable.

E.g. http://project1.dev/

```console
$ mysql -h project1.db.dev -u root
```

#### Additional documentation you may find useful.

 - [Docker-compose Development Environment](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/docker-compose-dev.md)
 - ["File-less" docker-compose](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/file-less-docker-compose.md)
