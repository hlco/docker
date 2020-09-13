# 基础镜像
FROM alpine:3.5

# 作者信息
MAINTAINER PHP HL "632968802@qq.com"

ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c

RUN apk add --no-cache \
		ca-certificates \
		curl \
		tar \
		xz \
		openssl

ENV PHP_INI_DIR /usr/local/etc/php

RUN set -eux; \
	addgroup -g 1000 -S www; \
	adduser -u 1000 -D -S -G www www; \
	mkdir -p "$PHP_INI_DIR/conf.d"; \
	[ ! -d /var/www/html ]; \
	mkdir -p /var/www/html; \
	chown www:www /var/www/html; \
	chmod 777 /var/www/html

ENV PHP_EXTRA_CONFIGURE_ARGS --enable-fpm --with-fpm-user=www --with-fpm-group=www --disable-cgi
ENV PHP_CFLAGS="-fstack-protector-strong -fpic -fpie -O2 -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64"
ENV PHP_CPPFLAGS="$PHP_CFLAGS"
ENV PHP_LDFLAGS="-Wl,-O1 -pie"
ENV GPG_KEYS 42670A7FE4D0441C8E4632349E4FDC074A4EF02D 5A52880781F755608BF815FC910DEB46F53EA312

ENV PHP_VERSION 7.4.10
ENV PHP_URL="https://www.php.net/distributions/php-7.4.10.tar.xz" PHP_ASC_URL="https://www.php.net/distributions/php-7.4.10.tar.xz.asc"
ENV PHP_SHA256="c2d90b00b14284588a787b100dee54c2400e7db995b457864d66f00ad64fb010" PHP_MD5=""

RUN set -eux; \
	\
	apk add --no-cache --virtual .fetch-deps gnupg; \
	\
	mkdir -p /usr/src; \
	cd /usr/src; \
	\
	curl -fsSL -o php.tar.xz "$PHP_URL"; \
#	\
#	if [ -n "$PHP_SHA256" ]; then \
#		echo "$PHP_SHA256 *php.tar.xz" | sha256sum -c -; \
#	fi; \
#	if [ -n "$PHP_MD5" ]; then \
#		echo "$PHP_MD5 *php.tar.xz" | md5sum -c -; \
#	fi; \
#	\
#	if [ -n "$PHP_ASC_URL" ]; then \
#		curl -fsSL -o php.tar.xz.asc "$PHP_ASC_URL"; \
#		export GNUPGHOME="$(mktemp -d)"; \
#		for key in $GPG_KEYS; do \
#			gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
#		done; \
#		gpg --batch --verify php.tar.xz.asc php.tar.xz; \
#		gpgconf --kill all; \
#		rm -rf "$GNUPGHOME"; \
#	fi; \
	\
	apk del --no-network .fetch-deps

#RUN set -eux; \
#	apk add --no-cache --virtual .build-deps \
#		$PHPIZE_DEPS \
#		argon2-dev \
#		coreutils \
#		curl-dev \
#		libedit-dev \
#		libsodium-dev \
#		libxml2-dev \
#		linux-headers \
#		oniguruma-dev \
#		openssl-dev \
#		sqlite-dev \
#	; \
#	\
#	export CFLAGS="$PHP_CFLAGS" \
#		CPPFLAGS="$PHP_CPPFLAGS" \
#		LDFLAGS="$PHP_LDFLAGS" \
#	; \
#	cd /usr/src/php; \
#	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
#	./configure \
#		--build="$gnuArch" \
#		--with-config-file-path="$PHP_INI_DIR" \
#		--with-config-file-scan-dir="$PHP_INI_DIR/conf.d" \
#		\
#		--enable-option-checking=fatal \
#		\
#		--with-mhash \
#		\
#		--enable-ftp \
#		--enable-mbstring \
#		--enable-mysqlnd \
#		--with-password-argon2 \
#		--with-sodium=shared \
#		--with-pdo-sqlite=/usr \
#		--with-sqlite3=/usr \
#		\
#		--with-curl \
#		--with-libedit \
#		--with-openssl \
#		--with-zlib \
#		\
#		--with-pear \
#		\
#		$(test "$gnuArch" = 's390x-linux-musl' && echo '--without-pcre-jit') \
#		\
#		${PHP_EXTRA_CONFIGURE_ARGS:-} \
#	; \
#	make -j "$(nproc)"; \
#	find -type f -name '*.a' -delete; \
#	make install; \
#	find /usr/local/bin /usr/local/sbin -type f -perm +0111 -exec strip --strip-all '{}' + || true; \
#	make clean; \
#	\
#	cp -v php.ini-* "$PHP_INI_DIR/"; \
#	\
#	runDeps="$( \
#		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
#			| tr ',' '\n' \
#			| sort -u \
#			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
#	)"; \
#	apk add --no-cache $runDeps; \
#	\
#	apk del --no-network .build-deps; \
#	\
#	pecl update-channels; \
#	rm -rf /tmp/pear ~/.pearrc; \
#	\
#	php --version

WORKDIR /var/www/html

RUN set -eux; \
	cd /usr/local/etc; \
	if [ -d php-fpm.d ]; then \
		sed 's!=NONE/!=!g' php-fpm.conf.default | tee php-fpm.conf > /dev/null; \
		cp php-fpm.d/www.conf.default php-fpm.d/www.conf; \
	else \
		mkdir php-fpm.d; \
		cp php-fpm.conf.default php-fpm.d/www.conf; \
		{ \
			echo '[global]'; \
			echo 'include=etc/php-fpm.d/*.conf'; \
		} | tee php-fpm.conf; \
	fi; \
	{ \
		echo '[global]'; \
		echo 'error_log = /proc/self/fd/2'; \
		echo; echo '; https://github.com/docker-library/php/pull/725#issuecomment-443540114'; echo 'log_limit = 8192'; \
		echo; \
		echo '[www]'; \
		echo '; if we send this to /proc/self/fd/1, it never appears'; \
		echo 'access.log = /proc/self/fd/2'; \
		echo; \
		echo 'clear_env = no'; \
		echo; \
		echo '; Ensure worker stdout and stderr are sent to the main error log.'; \
		echo 'catch_workers_output = yes'; \
		echo 'decorate_workers_output = no'; \
	} | tee php-fpm.d/docker.conf; \
	{ \
		echo '[global]'; \
		echo 'daemonize = no'; \
		echo; \
		echo '[www]'; \
		echo 'listen = 9000'; \
	} | tee php-fpm.d/zz-docker.conf

STOPSIGNAL SIGQUIT

EXPOSE 9000
CMD ["php-fpm"]
