FROM php:8.0.30-fpm-alpine3.16
LABEL maintainer="yangxiuchuan<757791723@qq.com>"
WORKDIR /var/www

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN set -xue; \
    apk add --no-cache freetype libjpeg-turbo libpng libwebp libzip rabbitmq-c libmcrypt libxml2 libstdc++ supervisor && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS freetype-dev libevent-dev libjpeg-turbo-dev libmcrypt-dev libpng-dev libxml2-dev libzip-dev libwebp-dev rabbitmq-c-dev linux-headers openssl-dev && \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} bcmath exif gd pcntl pdo_mysql mysqli zip sockets && \
    pecl install redis-6.0.2 && \
    pecl install mongodb-1.17.2 && \
    pecl install mcrypt-1.0.7 && \
    pecl install --configureoptions 'enable-openssl="yes" enable-sockets="yes" enable-mysqlnd="yes"' swoole-4.8.13 && \
    pecl install amqp-2.1.2 && \
    docker-php-ext-enable redis mongodb mcrypt swoole amqp && \
    docker-php-source delete && \
    apk del .build-deps && \
    rm -rf /tmp/* /var/cache/apk/* /tmp/pear ~/.pearrc

RUN set -xue;php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer
