ARG VERSION=latest
FROM alpine:${VERSION}
LABEL author="HL <632968802@qq.com>"

# 安装基础 package
RUN set -x \
#    && adduser -D -u 1000 -g 1000 -s /bin/sh www; \
#    && mkdir -p /data/site /data/log /data/conf /data/run /data/sessions; \
#    && chown -R www:www /data; \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update \
    && apk add --no-cache tzdata vim openssl openssh openssl-dev bash curl \
    # 变更时区
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata

# 安装 PHP
RUN set -x \
    && apk update \
    && apk add --no-cache php7 php7-fpm php7-pear php7-dev \
        php7-curl php7-json php7-phar php7-iconv php7-openssl php7-dom php7-mbstring \
        php7-mysqlnd php7-mysqli php7-pdo_mysql php7-simplexml php7-xmlrpc php7-xmlreader \
        php7-xmlwriter php7-opcache php7-zlib php7-zip php7-gd php7-session php7-redis \
        php7-memcached php7-tokenizer php7-fileinfo php7-ctype php7-bcmath php7-mcrypt \
        php7-bz2 php7-exif php7-gettext php7-ftp php7-pcntl php7-posix php7-shmop php7-sockets \
        php7-exif php7-sysvmsg php7-sysvsem php7-sysvshm php7-wddx php7-xsl

# 安装 & 配置 composer
RUN set -x \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin \
    && mv /usr/local/bin/composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 安装 php-memcache
ARG MEMCACHE_VERSION=4.0.4
ARG MEMCACHE_DIR=/data/pecl-memcache-${MEMCACHE_VERSION}
ADD https://github.com/websupport-sk/pecl-memcache/archive/${MEMCACHE_VERSION}.tar.gz /data/
RUN set -x \
    && apk update \
    # 安装编译需要依赖的库
    && apk add --no-cache gcc make linux-headers libc-dev \
    && cd /data \
    && tar -xf ${MEMCACHE_VERSION}.tar.gz \
    && rm -rf ${MEMCACHE_VERSION}.tar.gz \
    && cd ${MEMCACHE_DIR} \
    && phpize \
    && ./configure --with-php-config=/usr/bin/php-config --disable-memcache-session \
    && make && make install \
    && echo "extension=memcache.so" >> /etc/php7/php.ini \
    # 清除缓存、删除无用 package
    && make clean all \
    && apk del gcc make linux-headers libc-dev \
    && rm -rf ${MEMCACHE_DIR}:

EXPOSE 9000
CMD [ "/bin/sh" ]
