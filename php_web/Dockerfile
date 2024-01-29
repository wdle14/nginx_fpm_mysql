FROM wdle14/fpm_nginx:7.4.30-focal

ARG PROXY
ARG NO_PROXY

ENV http_proxy $PROXY
ENV https_proxy $PROXY
ENV no_proxy $NO_PROXY

ENV DEBIAN_FRONTEND noninteractive

WORKDIR /var/www/html


# Configure supervisor
COPY ./php/supervisord.ini /etc/supervisor/conf.d/supervisord.conf

# Configure php-fpm
RUN mv /etc/php/7.4/fpm/php-fpm.conf /etc/php/7.4/fpm/php-fpm.conf.ori
RUN mv /etc/php/7.4/fpm/pool.d/www.conf   /etc/php/7.4/fpm/pool.d/www.conf.ori 

COPY ./php/php-fpm.conf /etc/php/7.4/fpm/php-fpm.conf
COPY ./php/www.conf /etc/php/7.4/fpm/pool.d/www.conf


RUN mkdir -p /var/run/php/ && chmod -R 777 /var/run/php
# RUN touch /var/run/php/php8.0-fpm.pid
# RUN touch /var/run/php/php8.0-fpm.sock

COPY ./php/99-custom.ini /etc/php/7.4/cli/conf.d/99-custom.ini

COPY ./php/99-custom.ini /etc/php/7.4/fpm/conf.d/99-custom.ini



# Configure nginx
RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.ori
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./nginx/site.conf /etc/nginx/conf.d/default.conf

RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

RUN mkdir -p /laravel/log/swoole/
RUN chmod 777 /laravel/log/swoole/

#run internal command like composer & run supervisor
CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

EXPOSE 80