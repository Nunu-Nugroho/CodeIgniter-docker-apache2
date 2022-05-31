FROM php:5.6-apache

WORKDIR /var/www/html

RUN apt-get update && apt-get -y --no-install-recommends install \
    libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev \
    libmemcached-dev zlib1g-dev \
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
    && docker-php-ext-install mysqli \
    && docker-php-ext-enable mysqli\
    && docker-php-ext-install mysql pdo_mysql mbstring \
    && echo date.timezone=Asia/Jakarta >> /usr/local/etc/php/conf.d/php.ini \
    && echo upload_max_filesize = 100M >> /usr/local/etc/php/conf.d/php.ini \
    && echo post_max_size = 108M >> /usr/local/etc/php/conf.d/php.ini \
    && apt-get autoremove -y \
    && rm -rf /tmp/* /var/tmp/*

COPY .docker/default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod ssl && a2enmod rewrite
RUN mkdir -p /etc/apache2/ssl
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"


COPY ./.docker/ssl/localhost+2.pem /etc/apache2/ssl/cert.pem
COPY ./.docker/ssl/localhost+2-key.pem /etc/apache2/ssl/cert-key.pem


EXPOSE 80
EXPOSE 443