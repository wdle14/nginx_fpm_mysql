FROM alpine:latest

WORKDIR /var/www/html/

ARG PROXY

ENV http_proxy $PROXY
ENV https_proxy $PROXY

# Essentials
RUN echo "UTC" > /etc/timezone
RUN apk add --no-cache zip unzip curl sqlite nginx supervisor

# Installing bash
RUN apk add bash
RUN sed -i 's/bin\/ash/bin\/bash/g' /etc/passwd

# Installing PHP
RUN apk add --no-cache php7 \
    php7-common \
    php7-fpm \
    php7-pdo \
    php7-opcache \
    php7-zip \
    php7-phar \
    php7-iconv \
    php7-cli \
    php7-curl \
    php7-openssl \
    php7-mbstring \
    php7-tokenizer \
    php7-fileinfo \
    php7-json \
    php7-xml \
    php7-xmlwriter \
    php7-simplexml \
    php7-dom \
    php7-pecl-imagick \
    php7-pdo_mysql \
    php7-pdo_sqlite \
    php7-tokenizer \
    php7-ctype \
    php7-gd \
    php7-pgsql \
    php7-pdo_pgsql \
    php7-xmlreader \
    php7-pecl-redis

#RUN ln -s /usr/bin/php7 /usr/bin/php

# Installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN rm -rf composer-setup.php

# Configure supervisor
RUN mkdir -p /etc/supervisor.d/
COPY ./php/supervisord.ini /etc/supervisor.d/supervisord.ini

# Configure php-fpm
RUN mkdir -p /run/php/ && chmod -R 777 /run/php
RUN touch /run/php/php7.0-fpm.pid
RUN touch /run/php/php7.0-fpm.sock


COPY ./php/php-fpm.conf /etc/php7/php-fpm.conf
COPY ./php/99-custom.ini /etc/php7/conf.d/99-custom.ini
COPY ./php/www.conf /etc/php7/php-fpm.d/www.conf


# Configure nginx
#RUN echo "daemon off;" >> /etc/nginx/nginx.conf
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./nginx/site.conf /etc/nginx/http.d/default.conf
#COPY ./nginx/fastcgi-php.conf /etc/nginx/fastcgi-php.conf

RUN mkdir -p /run/nginx/
RUN touch /run/nginx/nginx.pid

RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

# Building process
#COPY . .
#RUN composer install --no-dev

# Configure Laravel logs
#RUN ln -sf /dev/stdout /var/www/html/storage/laravel.log

EXPOSE 80
CMD ["supervisord", "-c", "/etc/supervisor.d/supervisord.ini"]