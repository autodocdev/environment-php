FROM php:7.3-fpm-alpine3.10

LABEL maintaner="dcorrea@autodoc.com.br"

RUN set -x \
    && addgroup -g 1000 app \
    && adduser -u 1000 -D -G app app

WORKDIR /home/app/php

RUN apk add --no-cache \
    zip \
    libzip-dev \
    libxml2-dev \
    libedit-dev \
    libxslt-dev \
    libpng-dev \
    freetype-dev \
    libjpeg-turbo-dev \
    libressl-dev

ENV EXTENSION_PHP \
    zip \
    pdo \
    pdo_mysql \
    xml \
    json \
    opcache \
    readline \
    sockets \
    tokenizer \
    xsl \
    gd \
    mbstring \
    pcntl \
    phar

RUN docker-php-ext-install ${EXTENSION_PHP} \
    && docker-php-ext-configure zip --with-libzip=/usr/include \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-jpeg-dir \
        --with-png-dir \
        --with-zlib-dir

COPY *.conf /usr/local/etc/php-fpm.d/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && chown -R app:app /home/app

EXPOSE 9000

USER app

CMD ["php-fpm", "--nodaemonize"]
