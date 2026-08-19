FROM php:8.1.27-fpm-alpine3.19
MAINTAINER yangxiuchuan<757791723@qq.com>
WORKDIR /var/www
ENV EXT_DEPS freetype-dev libevent-dev libjpeg-turbo-dev libmcrypt-dev libpng-dev libxml2-dev libzip-dev libwebp-dev libxpm-dev rabbitmq-c-dev supervisor
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN set -xue;apk --no-cache update && apk --no-cache upgrade && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS $EXT_DEPS && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && docker-php-ext-install -j${NPROC} bcmath exif gd pcntl pdo_mysql mysqli zip sockets
RUN set -xue;pecl install redis-6.0.2 && pecl install mongodb-1.17.2 && pecl install mcrypt-1.0.7 && pecl install --configureoptions 'enable-openssl="yes"' swoole-4.8.13 && pecl install amqp-2.1.2 && docker-php-ext-enable redis mongodb mcrypt swoole amqp
RUN set -xue;php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer
RUN rm -rf /tmp/* /var/cache/apk/*
