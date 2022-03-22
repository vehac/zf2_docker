FROM php:5.6-apache

# install modules
RUN apt-get update && apt-get install -y \
    git zlib1g-dev libxml2-dev libmcrypt-dev curl libcurl4-gnutls-dev libssl-dev \
    mariadb-client openssl libmemcached11 libmemcachedutil2 build-essential libmemcached-dev libz-dev vim nano zip unzip

# install mongodb ext
RUN pecl install mongo && docker-php-ext-enable mongo
RUN pecl install mongodb-1.7.4 && docker-php-ext-enable mongodb

# install ext php
RUN docker-php-ext-configure intl && docker-php-ext-install intl \
&& docker-php-ext-install zip pdo pdo_mysql mysqli mbstring xml \
&& docker-php-ext-install soap && docker-php-ext-install mcrypt \
&& docker-php-ext-install mbstring && docker-php-ext-install curl \
&& docker-php-ext-install bcmath && docker-php-ext-install sockets

# active a2enmod and install composer
RUN a2enmod rewrite && a2enmod ssl && a2enmod headers \
    && curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer

# install ext memcached and clean the image
RUN pecl install memcached-2.2.0 \
&& echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini
RUN rm -rf /var/lib/apt/lists/* /tmp/*

ADD ./localhost.conf /etc/apache2/sites-enabled/000-default.conf

EXPOSE 80

WORKDIR /var/www/html