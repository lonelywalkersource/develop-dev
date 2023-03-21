FROM php:7.4.33-fpm

LABEL maintainer="Walker-Distance"

USER root
# 更换源为国内镜像
ADD sources.txt /etc/apt/

RUN apt-get update -y  \
    && apt-get install -y procps \
    && apt-get install -y git \
    && apt-get install -y libz-dev libzip-dev libfreetype6-dev libmcrypt-dev libjpeg-dev libpng-dev libwebp-dev \
    && apt-get install -y net-tools \
    && apt-get install -y libssl-dev

RUN pecl channel-update pecl.php.net \
    && docker-php-ext-configure zip \
    && docker-php-ext-configure gd --with-freetype=/usr/include --with-jpeg=/usr/include --with-webp=/usr/include \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo \
    && docker-php-ext-install sockets \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install https://pecl.php.net/get/swoole-4.8.11.tgz \
    && docker-php-ext-enable swoole \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /temp/pear/* /var/log/lastlog /var/log/faillog

WORKDIR /var/www

# 创建 www-data 用户 或者是 www 用户
RUN usermod -u 1000 -s /bin/bash www-data

USER www-data

CMD ["php-fpm"]

EXPOSE 9000
