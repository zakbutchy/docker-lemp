FROM node:20-buster as node
FROM php:8.1-fpm-buster

EXPOSE 5173

ENV TZ=Asia/Tokyo \
    # locale
    LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8

RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    git \
    unzip \
    vim \
    libzip-dev \
    libicu-dev \
    libonig-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install intl pdo_mysql zip bcmath \
    # xdebug install
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

# composer command
COPY --from=composer:2.5 /usr/bin/composer /usr/bin/composer
# node command
COPY --from=node /usr/local/bin /usr/local/bin
# npm command
COPY --from=node /usr/local/lib /usr/local/lib
# yarn command
COPY --from=node /opt /opt

# composer environment
ENV COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_HOME=/composer

COPY ./docker/php/php.ini /usr/local/etc/php/php.ini
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www/html

ENTRYPOINT [ "bash", "-c", "npm install; exec php-fpm" ]
