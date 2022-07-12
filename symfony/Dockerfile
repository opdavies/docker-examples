ARG PHP_VERSION=8.1

FROM php:${PHP_VERSION}-fpm-bullseye AS base

###

FROM base AS build

ENV PATH=${PATH}:/var/www/html/vendor/bin

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

RUN useradd --create-home app \
  && chown app:app -R /var/www/html \
  && apt-get update -yqq \
  && apt-get install -yqq \
    git \
    unzip

ENV APP_DEBUG=0
ENV APP_ENV=prod

COPY --chown=app:app .env* composer.* ./
COPY --chown=app:app bin bin
COPY --chown=app:app config config
COPY --chown=app:app public public
COPY --chown=app:app src src

USER app

RUN composer install --no-dev --prefer-dist --optimize-autoloader

COPY --chown=app:app . .

###

FROM build AS test

ENV APP_ENV=test

RUN composer install

RUN phpunit --testdox

###

FROM build AS production

RUN composer install --no-dev --prefer-dist --optimize-autoloader
