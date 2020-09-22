FROM php:7.2-fpm-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update \
    && apk add --no-cache libmcrypt-dev libpng-dev freetype-dev libjpeg-turbo-dev curl-dev git \
    && docker-php-ext-install curl pdo_pgsql pdo_sqlite mysqli pdo pdo_mysql mbstring bcmath zip opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/  \
    && docker-php-ext-install -j$(nproc) gd

EXPOSE 9000
CMD ["php-fpm"]