# Supported tags and respective `Dockerfile` links

 - [`5.6-platform`, `5.6-ssp`, `platform`, `ssp`, `latest` (*5.6/platform/Dockerfile*)](https://github.com/brettt89/silverstripe-web/blob/master/5.6/platform/Dockerfile)
 - [`5.6-platform-dev`, `5.6-ssp-dev`, `platform-dev`, `ssp-dev`, `latest-dev` (*5.6/platform/dev/Dockerfile*)](https://github.com/brettt89/silverstripe-web/blob/master/5.6/platform/dev/Dockerfile)
 - [`7.1-platform`, `7.1-ssp` (*7.1/platform/Dockerfile*)](https://github.com/brettt89/silverstripe-web/blob/master/7.1/platform/Dockerfile)
 - [`7.1-platform-dev`, `7.1-ssp-dev` (*7.1/platform/dev/Dockerfile*)](https://github.com/brettt89/silverstripe-web/blob/master/7.1/platform/dev/Dockerfile)

[Travis CI:
        ![Build Status](https://travis-ci.org/brettt89/silverstripe-web.svg?branch=master)](https://travis-ci.org/brettt89/silverstripe-web)

# How to use this image.

## Requirements

 - [*Docker Compose*](https://docs.docker.com/compose/install/)
 - [*Composer*](https://getcomposer.org/)
 - [*docker-compose.yml*](#examples)
 - [*\_ss\_environment.php*](#examples)

## Info

This image is extended from the [php5.6-apache](https://hub.docker.com/_/php/) image. It comes pre-packaged with a the default php extensions and configurations found on [SilverStripe Platform](https://platform.silverstripe.com). It also comes with some tooling pre-installed for ease-of-use.

For development environments / testing, we recommend using the `-dev` tagged image.

**Installed:**

 - [php](http://php.net/)
 - [Apache](https://www.apache.org/)
 - [Git](https://git-scm.com/)
 - [Composer](https://getcomposer.org/)
 - [sspak](https://github.com/silverstripe/sspak)

 **(Dev only)**

 - [xdebug](https://xdebug.org/)

### Common Folders

 - **Web directory:** `/var/www/html`
 - **Logs:** `/var/logs/`

## Additional Documentation

 - [Docker-compose development environment](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/docker-compose-dev.md)
 - [Using nginx-proxy for domains](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/nginx-proxy-domain.md)
 - [Using SSPak](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/using-sspak.md)
 - ["File-less" docker-compose](https://github.com/brettt89/silverstripe-web/blob/master/docs/en/file-less-docker-compose.md)


# License

View [license information](http://php.net/license/) for the software contained in this image.

# Supported Docker versions

This image is officially supported on Docker version 17.04.0-ce.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see [the Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.

# User Feedback

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/brettt89/silverstripe-web/issues).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/brettt89/silverstripe-web/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.

# Credits

 - Franco Springveldt - [https://github.com/fspringveldt](https://github.com/fspringveldt)
