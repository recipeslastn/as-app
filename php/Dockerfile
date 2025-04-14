FROM php:8.4.6-fpm-alpine as app

#RUN apt-get update && apt-get install -y    zlib1g-dev   lizip-dev    unzip

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN set -eux; \
# install-php-extensions pdo pdo_mysql zip;;
install-php-extensions pdo_mysql zip;

COPY --from=composer:2.4  /usr/bin/composer /usr/bin/composer

ENV COMPOSE_ALLOW_SUPERUSER=1



COPY ./app/composer.* ./

RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

COPY ./app .
# WORKDIR /the/workdir/path

RUN composer dump-autoload --optimize

FROM app as app_dev

ENV XDEBUG_MODE=off
COPY ./php/conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN set -eux; \
	install-php-extensions xdebug
