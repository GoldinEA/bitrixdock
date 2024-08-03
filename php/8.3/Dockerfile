FROM php:8.3-fpm-alpine

# Установка основных пакетов
RUN apk update && apk upgrade && \
    apk add --no-cache \
        curl \
        nginx \
        php83 \
        php83-ctype \
        php83-curl \
        php83-dom \
        php83-fileinfo \
        php83-fpm \
        php83-gd \
        php83-intl \
        php83-mbstring \
        php83-mysqli \
        php83-opcache \
        php83-openssl \
        php83-phar \
        php83-session \
        php83-tokenizer \
        php83-xml \
        php83-xmlreader \
        php83-xmlwriter \
        sudo \
        imagemagick \
        imagemagick-dev \
        zlib-dev \
        libmemcached-dev \
        cyrus-sasl-dev

# Установка пакетов для сборки
RUN apk add --no-cache --virtual .build-deps \
        gcc \
        g++ \
        make \
        autoconf \
        linux-headers \
        pkgconfig \
        zlib-dev \
        libmemcached-dev \
        cyrus-sasl-dev

# Установка и запуск Memcached
RUN apk add --no-cache memcached

# Установка расширений Imagick, Xdebug, Memcache и Memcached
RUN pecl install imagick xdebug memcache memcached && \
    docker-php-ext-enable imagick xdebug memcache memcached

# Удаление пакетов для сборки
RUN apk del .build-deps && \
    rm -rf /var/cache/apk/*

# Копирование настроек PHP
COPY ./php.ini /etc/php83/conf.d/php.ini
RUN docker-php-ext-install mysqli

# Создание пользователя и группы www-data, если они не существуют
RUN if ! getent group www-data; then addgroup -S www-data; fi \
    && if ! getent passwd www-data; then adduser -S -G www-data www-data; fi

# Установка прав на запуск команд от имени www-data
RUN echo 'www-data ALL=(ALL) NOPASSWD: /usr/bin/memcached' >> /etc/sudoers

WORKDIR "/var/www/bitrix"

EXPOSE 9000

# Запуск Memcached и PHP-FPM под пользователем www-data с использованием sudo
CMD ["sh", "-c", "sudo -u www-data memcached -u www-data -d && php-fpm"]
