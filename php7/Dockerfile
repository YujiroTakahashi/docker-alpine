FROM alpine:3.4

# ユーザー・グループの追加
RUN set -xe \
    && addgroup -g 1000 -S croco \
    && adduser -u 1000 -D -S -G croco croco \

# 標準Cライブラリのインストール
    && apk --update add \
           --virtual .build-deps \
        musl \
        musl-dev \

# 各種ツール・コンパイル用ライブラリのインストール
    && apk --update add \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
           --virtual .build-deps \
        autoconf \
        cyrus-sasl-dev \
        curl \
        g++ \
        gcc \
        git \
        gzip \
        libc-dev \
        libtool \
        libevent-dev \
        libsasl \
        linux-headers \
        make \
        openldap-dev \
        openssl-dev \
        pcre-dev \
        php7-dev \
        tar \
        unzip \
        zlib-dev \

# 恒久的なパッケージのインストール
    && apk --update add --no-cache \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
           --virtual .persistent-deps \

# サービス・ライブラリのインストール
        libldap \
        libstdc++ \
        openssl \
        pcre \
        supervisor \
        zlib \

# PHPと各moduleのインストール
        php7 \
        php7-bcmath \
        php7-bz2 \
        php7-calendar \
        php7-common \
        php7-ctype \
        php7-curl \
        php7-fpm \
        php7-gettext \
        php7-json \
        php7-ldap \
        php7-mbstring \
        php7-mcrypt \
        php7-mysqlnd \
        php7-opcache \
        php7-openssl \
        php7-pdo \
        php7-pdo_mysql \
        php7-phar \
        php7-posix \
        php7-session \
        php7-sockets \
        php7-stats \
        php7-uuid \
        php7-zip \
        php7-zlib \

# amazon-elasticache-clusterのインストール
    && git clone git://github.com/Yujiro3/aws-elasticache-cluster-client-libmemcached /tmp/aws-elasticache-cluster-client-libmemcached \
    && mkdir /tmp/aws-elasticache-cluster-client-libmemcached/BUILD \
    && cd /tmp/aws-elasticache-cluster-client-libmemcached/BUILD \
    && ../configure --prefix=/usr --with-pic \
    && make \
    && make install \

# phalconのインストール
    && git clone -b 3.0.x git://github.com/phalcon/cphalcon.git /tmp/cphalcon \
    && cd /tmp/cphalcon/build/php7/64bits \
    && phpize7 \
    && ./configure CC="gcc" CFLAGS="-march=native -mtune=native -O2 -fomit-frame-pointer -fvisibility=hidden" CPPFLAGS="-DPHALCON_RELEASE" --with-php-config=/usr/bin/php-config7 --enable-phalcon \
    && make \
    && make install \
    && echo "extension=phalcon.so" > /etc/php7/conf.d/60_phalcon.ini \

# igbinaryのインストール
    && curl -fSL "https://github.com/igbinary/igbinary/archive/php7-dev-playground2.zip" -o "/tmp/igbinary.zip" \
    && unzip -q /tmp/igbinary.zip -d /tmp \
    && cd /tmp/igbinary-php7-dev-playground2 \
    && phpize7 \
    && ./configure CC="gcc" CFLAGS="-O2 -g" --with-php-config=/usr/bin/php-config7 --enable-igbinary \
    && make \
    && make install \
    && echo "extension=igbinary.so" > /etc/php7/conf.d/00_igbinary.ini \

# aws-elasticache-cluster-client-memcached-for-phpのインストール
    && git clone -b php7 git://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php.git /tmp/aws-elasticache-cluster-client-memcached-for-php \
    && cd /tmp/aws-elasticache-cluster-client-memcached-for-php \
    && phpize7 \
    && ./configure CC="gcc" CFLAGS="-O2" \
        --with-php-config=/usr/bin/php-config7 --with-libmemcached-dir=/usr \
        --disable-memcached-sasl --enable-memcached-igbinary --enable-memcached \
    && make \
    && make install \
    && echo "extension=memcached.so" > /etc/php7/conf.d/30_memcached.ini \


# コンパイルに使ったファイルの削除
    && rm -rf /tmp/* \
    && apk del .build-deps \

# アクセス権限の変更
    && chown -R croco:croco /var/log/php7 \
    && mkdir -p /var/run/php-fpm7 \
    && chown -R croco:croco /var/run/php-fpm7

# ファイルの追加
ADD opt /opt

EXPOSE 9000
CMD ["php-fpm7", "--fpm-config", "/opt/conf/php-fpm.conf", "--nodaemonize"]
