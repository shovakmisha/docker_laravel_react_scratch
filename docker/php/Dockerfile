FROM php:8.0-fpm

RUN apt-get update && \
    apt-get install -y \
    git \
    sudo

RUN pecl channel-update pecl.php.net \
    && pecl install xdebug \
    && docker-php-ext-install mysqli pdo pdo_mysql \
    && docker-php-ext-enable pdo_mysql

RUN docker-php-ext-enable xdebug

COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

COPY conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Add a non-root user to prevent files being created with root permissions on host machine
ARG uid=1000
ARG gid=1000

RUN groupadd -g ${gid} appuser && \
   useradd -u ${uid} -g appuser -m appuser && \
   echo "appuser:password" | chpasswd && \
   apt-get update && \
   apt-get install -y sudo && \
   usermod -aG sudo appuser

WORKDIR /var/www/app/laravel

