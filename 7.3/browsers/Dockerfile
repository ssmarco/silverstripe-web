FROM circleci/php:7.3-apache-browsers-legacy
MAINTAINER Brett Tasker "<brett@silverstripe.com>"
ENV DEBIAN_FRONTEND=noninteractive

USER root

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils \
        curl \
		git-core \
		gzip \
		openssh-client \
		unzip \
		zip

# Nodejs
RUN curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -
RUN echo "deb https://deb.nodesource.com/node_10.x buster main" | tee /etc/apt/sources.list.d/nodesource.list
RUN echo "deb-src https://deb.nodesource.com/node_10.x buster main" | tee -a /etc/apt/sources.list.d/nodesource.list

# Yarn
RUN curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Install components
RUN apt-get update && sudo apt-get install -y \
		nodejs \
		yarn \
	--no-install-recommends && \
	apt-get autoremove -y && \
	rm -rf /var/lib/apt/lists/*

# Install default PHP Extensions
RUN docker-php-ext-install -j$(nproc) \
		bcmath \
		mysqli \
		pdo \
		pdo_mysql

# Install Intl, LDAP, GD, SOAP, Tidy, XSL, Zip PHP Extensions
RUN apt-get update -y && apt-get install -y \
		zlib1g-dev \
        libicu-dev \
        g++ \
        libldap2-dev \
        libgd-dev \
        libzip-dev \
        libtidy-dev \
        libxml2-dev \
        libxslt-dev \
    --no-install-recommends && \
    apt-mark auto \
        zlib1g-dev \
        libicu-dev \
        g++ \
        libldap2-dev \
        libxml2-dev \
        libxslt-dev && \
    docker-php-ext-configure intl && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) \
        intl \
        ldap \
        gd \
        soap \
        tidy \
        xsl \
        zip && \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false && \
	rm -rf /var/lib/apt/lists/*

# Apache + xdebug configuration
RUN { \
                echo "<VirtualHost *:80>"; \
                echo "  DocumentRoot /var/www/html"; \
                echo "  LogLevel warn"; \
                echo "  ErrorLog /var/log/apache2/error.log"; \
                echo "  CustomLog /var/log/apache2/access.log combined"; \
                echo "  ServerSignature Off"; \
                echo "  <Directory /var/www/html>"; \
                echo "    Options +FollowSymLinks"; \
                echo "    Options -ExecCGI -Includes -Indexes"; \
                echo "    AllowOverride all"; \
                echo; \
                echo "    Require all granted"; \
                echo "  </Directory>"; \
                echo "  <LocationMatch assets/>"; \
                echo "    php_flag engine off"; \
                echo "  </LocationMatch>"; \
                echo; \
                echo "  IncludeOptional sites-available/000-default.local*"; \
                echo "</VirtualHost>"; \
	} | tee /etc/apache2/sites-available/000-default.conf


RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/fqdn.conf && \
	echo "date.timezone = Pacific/Auckland" | tee /usr/local/etc/php/conf.d/timezone.ini && \
    printf "log_errors = On\nerror_log = /dev/stderr\n" | tee /usr/local/etc/php/conf.d/errors.ini && \
	a2enmod rewrite headers expires remoteip cgid && \
	usermod -u 1000 www-data && \
	usermod -G staff www-data

EXPOSE 80

CMD ["apache2-foreground"]
