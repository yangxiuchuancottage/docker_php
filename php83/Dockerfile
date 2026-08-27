FROM php:8.3.33-fpm-alpine3.23
LABEL maintainer="yangxiuchuan<757791723@qq.com>"
WORKDIR /var/www

# 使用阿里云镜像加速
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 安装PECL扩展所需的系统库
RUN set -xue; \
    apk add --no-cache freetype libjpeg-turbo libpng libwebp libzip rabbitmq-c libstdc++ supervisor && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev libzip-dev rabbitmq-c-dev linux-headers openssl-dev && \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} bcmath exif gd pcntl pdo_mysql mysqli zip sockets && \
    pecl install redis-6.3.0 && \
    pecl install mongodb-2.4.0 && \
    pecl install --configureoptions 'enable-openssl="yes" enable-sockets="yes" enable-http2="yes" enable-mysqlnd="yes"' swoole-5.1.8 && \
    pecl install amqp-2.1.2 && \
    pecl install protobuf-5.36.0 && \
    docker-php-ext-enable redis mongodb swoole amqp protobuf && \
    docker-php-source delete && \
    apk del .build-deps && \
    rm -rf /tmp/* /var/cache/apk/* /tmp/pear ~/.pearrc


RUN set -xue; \
    php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer
