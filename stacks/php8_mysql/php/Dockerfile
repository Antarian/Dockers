FROM php:8.0-cli

LABEL phpmysqldevelop-dockerfile=PHPMYSQLDEVELOP

# prepare php ini settings
RUN mkdir /usr/src/php
RUN tar --file /usr/src/php.tar.xz --extract --strip-components=1 --directory /usr/src/php
RUN cp /usr/src/php/php.ini-development /usr/local/etc/php/php.ini

# add composer
COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer

# add git, wget, zip
RUN apt-get update \
  && apt-get install -y \
        curl \
        git \
        wget \
        libzip-dev \
        zip
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip

# BC Math extension
RUN docker-php-ext-install bcmath

# MySQL extension
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install and setup xDebug
RUN pecl install xdebug-3.0.2 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=debug" >> /usr/local/etc/php/php.ini \
    && echo 'xdebug.client_port=9003' >> /usr/local/etc/php/php.ini \
    && echo 'xdebug.discover_client_host=1' >> /usr/local/etc/php/php.ini

# Remove install files
RUN rm -rf /var/lib/apt/lists/*

# Symfony FW
RUN wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# PHP CS Fixer
RUN wget https://cs.symfony.com/download/php-cs-fixer-v2.phar -O php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

# PHP Infection
RUN wget https://github.com/infection/infection/releases/download/0.10.3/infection.phar -O infection
RUN chmod +x infection
RUN mv infection /usr/local/bin/infection

# create and change ownership of /.composer folder for composer cache
RUN mkdir -p /.composer
RUN chown 1000:1000 -R /.composer

# prepare workdir
RUN mkdir -p /usr/src/app;
WORKDIR /usr/src/app
