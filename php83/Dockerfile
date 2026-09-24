FROM php:8.3.33-fpm-trixie
LABEL maintainer="yangxiuchuan<757791723@qq.com>"
WORKDIR /var/www
RUN sed -i 's|http://deb.debian.org|https://mirrors.aliyun.com|g' /etc/apt/sources.list.d/debian.sources
RUN set -xue; \
    apt-get update; \
    apt-get install -y --no-install-recommends libfreetype6 libjpeg62-turbo libpng16-16t64 libwebp7 libwebpdemux2 libzip5 librabbitmq4 libstdc++6 supervisor; \
    apt-get install -y --no-install-recommends $PHPIZE_DEPS libfreetype-dev libjpeg62-turbo-dev libpng-dev libwebp-dev libzip-dev librabbitmq-dev libssl-dev; \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp; \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1); \
    docker-php-ext-install -j${NPROC} bcmath exif gd pcntl pdo_mysql mysqli zip sockets; \
    pecl install redis-6.3.0; \
    pecl install mongodb-2.4.0; \
    pecl install --configureoptions 'enable-openssl="yes" enable-sockets="yes" enable-http2="yes" enable-mysqlnd="yes"' swoole-5.1.8; \
    pecl install amqp-2.1.2; \
    pecl install protobuf-5.36.0; \
    docker-php-ext-enable redis mongodb swoole amqp protobuf; \
    docker-php-source delete; \
    apt-get purge -y --auto-remove $PHPIZE_DEPS libfreetype-dev libjpeg62-turbo-dev libpng-dev libwebp-dev libzip-dev librabbitmq-dev libssl-dev; \
    apt-get clean; \
    rm -rf /tmp/* /var/lib/apt/lists/* /tmp/pear ~/.pearrc
RUN set -xue; \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
    php -r "copy('https://composer.github.io/installer.sig', 'composer-setup.sig');"; \
    php -r "if (hash_file('sha384', 'composer-setup.php') !== trim(file_get_contents('composer-setup.sig'))) { unlink('composer-setup.php'); unlink('composer-setup.sig'); fwrite(STDERR, 'Invalid installer signature'.PHP_EOL); exit(1); }"; \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer; \
    php -r "unlink('composer-setup.php'); unlink('composer-setup.sig');"; \
    composer --version