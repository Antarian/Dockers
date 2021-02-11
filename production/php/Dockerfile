#
# Symfony 5, Nginx production Docker
#

# the different stages of this Dockerfile are meant to be built into separate images
# https://docs.docker.com/develop/develop-images/multistage-build/#stop-at-a-specific-build-stage
# https://docs.docker.com/compose/compose-file/#target

# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG PHP_VERSION=8.0

#
# Frontent dependencies
#
# uncomment if you are using Twig
#FROM node:latest as frontend
#
#RUN mkdir -p /app/public
#
#COPY package.json webpack.config.js yarn.lock /app/
#COPY assets/ /app/assets/
#
#WORKDIR /app
#
#RUN yarn install && yarn build

#
# PHP vendor dependencies
#
FROM composer:latest AS vendor

COPY composer.json composer.json
COPY composer.lock composer.lock

RUN composer install \
    --ignore-platform-reqs \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --no-dev \
    --prefer-dist

#
# PHP
#
FROM php:${PHP_VERSION}-fpm-alpine AS php_framework

# persistent / runtime deps
RUN apk add --no-cache \
		acl \
		fcgi \
		file \
		gettext
	;

ARG APCU_VERSION=5.1.18
RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		icu-dev \
		libzip-dev \
		postgresql-dev \
		zlib-dev \
	; \
	\
	docker-php-ext-configure zip; \
	docker-php-ext-install -j$(nproc) \
		intl \
		pdo_pgsql \
		zip \
	; \
	pecl install \
		apcu-${APCU_VERSION} \
	; \
	pecl clear-cache; \
	docker-php-ext-enable \
		apcu \
		opcache \
	; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --no-cache --virtual .api-phpexts-rundeps $runDeps; \
	\
	apk del .build-deps





# build for production
ARG APP_ENV=prod

WORKDIR /srv/app

# copy files to production image
# uncomments if console commands are used in production
# COPY bin bin/
COPY config config/
# uncomment if you are handling migrations through PHP and Doctrine
# COPY migrations migrations/
COPY public public/
COPY src src/
# uncomment if you are using Twig
# COPY templates templates/
COPY translations translations/
COPY --from=vendor /app/vendor/ vendor/
# uncomment if you are using Twig
COPY --from=frontend /app/public/build public/build
