FROM php:7.4-fpm-alpine

RUN apk update \
    && apk add  --no-cache git mysql-client curl libmcrypt libmcrypt-dev openssh-client icu-dev \
    libxml2-dev freetype-dev libpng-dev libjpeg-turbo-dev g++ make autoconf \
    && docker-php-source extract \
    && pecl install opcache xdebug redis apcu \
    && docker-php-ext-enable opcache xdebug redis apcu \
    && docker-php-source delete \
    && apk add --no-cache zip libzip-dev \
    && docker-php-ext-configure zip \
    && docker-php-ext-install pdo_mysql soap intl mysqli gd exif zip bcmath \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=10000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
#    && rm -rf /tmp/* \

# Setup GD extension
RUN docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
    --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir \
    --enable-gd-native-ttf

RUN docker-php-ext-install gd opcache

copy opcache.ini /usr/local/etc/php/conf.d

CMD ["php-fpm", "-F"]

WORKDIR /var/www/html

EXPOSE 9000
