FROM brettt89/silverstripe-web:7.1-platform
MAINTAINER Brett Tasker "<brett@silverstripe.com>"
ENV DEBIAN_FRONTEND=noninteractive

# Install components
RUN apt-get update -y && apt-get install -y \
		wget \
		git-core \
  --no-install-recommends && \
  pecl install xdebug && \
  rm -r /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-enable xdebug && \
  sed -i '1 a xdebug.remote_autostart=true' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_mode=req' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_handler=dbgp' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_connect_back=1 ' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_port=9000' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_host=127.0.0.1' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
  sed -i '1 a xdebug.remote_enable=1' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
