FROM php:7.4.1-fpm-alpine
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384','composer-setup.php')==='baf1608c33254d00611ac1705c1d9958c817a1a33bce370c0595974b342601bd80b92a3f46067da89e3b06bff421f182') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \\
 && php composer-setup.php --install-dir=/usr/local/bin --filename=composer && php -r "unlink('composer-setup.php');"
RUN apk add --update --no-cache --virtual .ext-deps \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        postgresql-dev \
        libzip-dev \
        supervisor \
        freetype-dev \
        libmcrypt-dev \
        autoconf \
        g++ \
        make \
        openssl-dev \
        icu-dev
RUN \
    docker-php-ext-configure pdo_mysql && \
    docker-php-ext-configure pdo_pgsql && \
    docker-php-ext-configure calendar && \
    docker-php-ext-configure zip && \
    docker-php-ext-configure opcache && \
    docker-php-ext-configure exif && \
    docker-php-ext-configure gd && \
    docker-php-ext-configure intl && \
    docker-php-ext-configure sockets && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-install pdo_mysql pdo_pgsql zip opcache exif gd intl sockets bcmath pcntl calendar

RUN \
    apk add --no-cache --virtual openssl-dev pcre-dev

RUN \
    pecl install redis && \
    pecl clear-cache && \
    docker-php-ext-enable redis && \
    docker-php-source delete
RUN mkdir -p ~/.config/psysh && chmod -R 755 ~/.config
COPY supervisord.conf /etc/supervisord.conf
USER "1000:1000"
ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]
