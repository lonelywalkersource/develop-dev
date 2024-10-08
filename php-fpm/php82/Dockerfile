FROM php:8.2.11-fpm

LABEL maintainer="walker-distance"

USER root

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

RUN apt-get update -y &&  apt-get install -y git libz-dev libzip-dev libfreetype6-dev libgmp-dev libmcrypt-dev libjpeg-dev libpng-dev libpq-dev libcurl4-openssl-dev libevent-dev libssh2-1-dev

RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable gd \
    && docker-php-ext-install exif \
    && docker-php-ext-install zip \
    && docker-php-ext-install gmp \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install sockets \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pgsql \
    && docker-php-ext-install pdo_pgsql \
    && pecl install swoole  \
    && docker-php-ext-enable swoole \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install ssh2 \
    && docker-php-ext-enable ssh2 \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-source delete

RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && rm -f composer-setup.php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer global config vendor-dir "/usr/local/composer-global/vendor"  \
    && composer global config bin-dir "/usr/local/composer-global/bin"  \
    && composer global require laravel/pint

COPY php.ini-development /usr/local/etc/php/php.ini

RUN apt-get clean && rm -rf /var/cache/apk/* && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 -s /bin/bash www-data

USER www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
