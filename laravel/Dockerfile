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

COPY --chown=app:app artisan composer.* ./
COPY --chown=app:app app app
COPY --chown=app:app bootstrap bootstrap
COPY --chown=app:app config config
COPY --chown=app:app routes routes

USER app

RUN composer install --no-dev --prefer-dist --optimize-autoloader

COPY --chown=app:app . .

###

FROM build AS test

RUN composer install

RUN cp -v --no-clobber .env.example .env \
  && php artisan key:generate \
  && phpunit --testdox

###

FROM build AS production

RUN composer install --no-dev --prefer-dist --optimize-autoloader
