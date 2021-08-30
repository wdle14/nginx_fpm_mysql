FROM php:7.3-fpm-alpine
# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN apk add --update --no-cache --virtual .ext-deps \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        postgresql-dev \
        libzip-dev \
        freetype-dev \
        libmcrypt-dev \
        autoconf \
        g++ \
        make \
        openssl-dev \
        icu-dev
RUN apk add --no-cache --upgrade bash

RUN apk add imagemagick

RUN apk add imagemagick-dev

RUN pecl install imagick \
    && docker-php-ext-enable imagick

RUN \
    docker-php-ext-configure pdo_mysql && \
    docker-php-ext-configure pdo_pgsql && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-configure opcache && \
    docker-php-ext-configure exif && \
    docker-php-ext-configure gd \
        --with-jpeg-dir=/usr/include \
        --with-png-dir=/usr/include \
        --with-webp-dir=/usr/include \
        --with-freetype-dir=/usr/include && \
    docker-php-ext-configure intl && \
    docker-php-ext-configure sockets && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-install pdo_mysql pdo_pgsql zip opcache exif gd intl sockets bcmath

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

COPY yy-custom.ini /usr/local/etc/php/conf.d/

RUN \
    apk add --no-cache --virtual openssl-dev pcre-dev

RUN \
    pecl install redis && \
    pecl clear-cache && \
    docker-php-ext-enable redis && \
    docker-php-source delete


# add bash script
# RUN mkdir /script
# COPY run.sh /script/run.sh
# RUN chmod +x /script/run.sh
#ADD run.sh /
#WORKDIR /var/www/html/
#CMD bash -c "composer install"
#run internal command like composer & run supervisor
#ENTRYPOINT [ "/run.sh" ]
