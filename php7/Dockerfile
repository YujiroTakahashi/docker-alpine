FROM alpine:latest

# ユーザー・グループの追加
RUN set -xe \
    && addgroup -g 1000 -S croco \
    && adduser -u 1000 -D -S -G croco croco \

    && apk add --no-cache --virtual .persistent-deps \
        nginx \
        supervisor \

    && apk --update add --no-cache \
           --repository http://nl.alpinelinux.org/alpine/edge/testing/ \
           --virtual .persistent-deps \
# PHPと各moduleのインストール
        php7 \
        php7-common \
        php7-curl \
        php7-fpm \
        php7-zlib \
        php7-pdo \
        php7-pdo_mysql \
        php7-gettext \
        php7-mcrypt \
        php7-mbstring \
        php7-phar \
        php7-posix \
        php7-ldap \
        php7-json \
        php7-opcache \

# コンパイルに必要なライブラリのインストール
    && apk --update add --no-cache \
           --repository http://nl.alpinelinux.org/alpine/edge/testing/ \
           --virtual .build-deps \
        curl \
        git \
        autoconf \
        file \
        gcc \
        make \
        libc-dev \
        php7-dev \

# amazon-elasticache-cluster-clientのインストール
    && curl -fSL "https://s3.amazonaws.com/elasticache-downloads/ClusterClient/PHP-7.0/latest-64bit" -o "/tmp/AmazonElastiCacheClusterClient.zip" \
    && unzip -q /tmp/AmazonElastiCacheClusterClient.zip -d /tmp \
    && cp /tmp/artifact/amazon-elasticache-cluster-client.so /usr/lib/php7/modules/memcached.so \
    && echo "extension=memcached.so" > /etc/php7/conf.d/00_memcached.ini \

# phalconのインストール
    && git clone git://github.com/phalcon/cphalcon.git /tmp/cphalcon \
    && cd /tmp/cphalcon \
    && git checkout 3.0.x \
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

# コンパイルに使ったファイルの削除
    && rm -rf /tmp/* \
    && apk del .build-deps

#EXPOSE 9000
#CMD ["php-fpm"]
