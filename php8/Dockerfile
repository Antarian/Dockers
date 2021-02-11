FROM php:8.2-cli
  
LABEL phpdevelop-dockerfile=PHPDEVELOP

# prepare php ini settings
RUN mkdir /usr/src/php \
    && tar --file /usr/src/php.tar.xz --extract --strip-components=1 --directory /usr/src/php \
    && cp /usr/src/php/php.ini-development /usr/local/etc/php/php.ini

# prepare dir for composer
RUN mkdir -p /.composer \
    && chown 1000:1000 -R /.composer

# add composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# add curl, git, wget, zip, sqlite, intl
RUN apt-get update \
  && apt-get install -y \
        curl \
        git \
        wget \
        libzip-dev \
        zip \
        libsqlite3-dev \
        libicu-dev
# Remove install files (not really required for dev)
# && rm -rf /var/lib/apt/lists/*

# Zip
RUN docker-php-ext-configure zip \
    && docker-php-ext-install zip

# SQLite
#RUN docker-php-ext-install pdo_sqlite \
#   && docker-php-ext-enable pdo_sqlite

# BC Math extension
#RUN docker-php-ext-install bcmath

# Mongo DB
#RUN pecl install mongodb-1.15.0 \
#    && docker-php-ext-enable mongodb

# Instal intl
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Install and setup xDebug
#RUN pecl install xdebug-3.2.0 \
#    && docker-php-ext-enable xdebug \
#    && echo "xdebug.mode=debug" >> /usr/local/etc/php/php.ini \
#    && echo 'xdebug.client_port=9003' >> /usr/local/etc/php/php.ini \
#    && echo 'xdebug.discover_client_host=1' >> /usr/local/etc/php/php.ini

# Symfony FW
RUN mkdir /.symfony5 && chmod a+rwx /.symfony5 \
    && curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash \
    && apt-get install symfony-cli
#    && mv /.symfony5/bin/symfony /usr/local/bin/symfony

# PHP CS Fixer
#RUN wget https://cs.symfony.com/download/php-cs-fixer-v3.phar -O php-cs-fixer \
#    && chmod a+x php-cs-fixer \
#    && mv php-cs-fixer /usr/local/bin/php-cs-fixer

# PHP Infection
#RUN wget https://github.com/infection/infection/releases/download/0.26.15/infection.phar -O infection \
#    && chmod +x infection \
#    && mv infection /usr/local/bin/infection

# prepare workdir
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
