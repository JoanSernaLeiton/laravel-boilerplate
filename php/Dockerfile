FROM php:8.2-fpm-alpine

# Instalar dependencias y extensiones PHP
RUN apk add --update --no-cache --virtual .build-deps \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        postgresql-dev \
        libzip-dev \
        icu-dev \
        freetype-dev \
        libmcrypt-dev \
        autoconf \
        g++ \
        make \
        openssl-dev \
        icu-dev \
        pcre-dev \
        linux-headers \
    && apk add --no-cache \
        supervisor \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        pdo_pgsql \
        zip \
        opcache \
        exif \
        gd \
        intl \
        sockets \
        bcmath \
        pcntl \
        calendar \
    && pecl install redis mongodb \
    && docker-php-ext-enable redis mongodb sodium \
    && rm -rf /tmp/pear \
    && docker-php-source delete

# Configurar directorio para PsySH
RUN mkdir -p /home/www-data/.config/psysh && chmod -R 755 /home/www-data/.config

# Cambiar al usuario www-data
USER www-data

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

# Command to remove dependencies [with this command no working]
# && apk del .build-deps \
