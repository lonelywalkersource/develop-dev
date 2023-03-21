FROM php:8.1-fpm

LABEL maintainer="Walker-Distance"

USER root
# 更换源为国内镜像
ADD sources.txt /etc/apt/

RUN apt-get update -y && apt-get install -y zlib1g=1:1.2.8.dfsg-5 --allow-downgrades \
    && apt-get install -y  git libz-dev libzip-dev libfreetype6-dev libmcrypt-dev  libjpeg-dev libpng-dev libwebp-dev

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
