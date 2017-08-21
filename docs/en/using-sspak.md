## Using SSPAK

SSPAK is loaded onto the web container by default. You can mount an sspak or sspak directory directly onto the ***web*** box by adding additional [*volume's*](https://docs.docker.com/compose/compose-file/#volumes) to your `docker-compose.yml`.

```yml
    volumes:
      - .:/var/www/html
      - ~/sspaks:/var/www/sspaks
```

Then you can run your `sspak` commands on the ***web*** container.

```console
$ sspak load /var/www/sspaks/project1-db-assets.sspak /var/www/html
```

```console
$ sspak save /var/www/html /var/www/sspaks/project1-db-assets.sspak
```
