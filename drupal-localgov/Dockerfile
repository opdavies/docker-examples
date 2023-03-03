FROM php:8.1-fpm-bullseye AS base

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
RUN which composer && composer -V

ARG DOCKER_UID=1000
ENV DOCKER_UID="${DOCKER_UID}"

WORKDIR /app

RUN adduser --disabled-password --uid "${DOCKER_UID}" app \
  && chown app:app -R /app

USER app

ENV PATH="${PATH}:/app/vendor/bin"

COPY --chown=app:app composer.* ./

################################################################################

FROM base AS build

USER root

RUN apt-get update -yqq \
  && apt-get install -yqq --no-install-recommends \
    git libpng-dev libzip-dev mariadb-client unzip

RUN docker-php-ext-install gd pdo_mysql zip

COPY --chown=app:app assets assets

USER app

RUN composer validate --strict
RUN composer install

COPY --chown=app:app tools/docker/images/php/root /

ENTRYPOINT ["/usr/local/bin/docker-entrypoint-php"]
CMD ["php-fpm"]

################################################################################

FROM base AS test

COPY --chown=app:app . .

RUN parallel-lint src --no-progress \
  && phpcs -vv \
  && phpstan \
  && phpunit --testdox



################################################################################

FROM nginx:1 as web

EXPOSE 8080

WORKDIR /app

COPY tools/docker/images/web/root /
