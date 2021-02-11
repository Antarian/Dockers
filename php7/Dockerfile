FROM php:7.4-cli
  
LABEL php7develop-dockerfile=PHP7DEVELOP

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
        zip \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libsqlite3-dev \
        libicu-dev \
    && rm -rf /var/lib/apt/lists/*

# Zip
RUN docker-php-ext-configure zip \
    && docker-php-ext-install zip

# GD
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install gd

# SQLite
RUN docker-php-ext-install pdo_sqlite \
    && docker-php-ext-enable pdo_sqlite

# BC Math extension
RUN docker-php-ext-install bcmath

# Mongo DB
RUN pecl install mongodb-1.9.0 \
    && docker-php-ext-enable mongodb

# Instal intl
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Install and setup xDebug
RUN pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini \
    && echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/php.ini \
    && echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini

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

# prepare dir for composer
RUN mkdir -p /.composer
RUN chown 1000:1000 -R /.composer

# prepare workdir
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
