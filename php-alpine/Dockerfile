FROM alpine:3.4

# ユーザー・グループの追加
RUN set -xe \
    && addgroup -g 1000 -S croco \
    && adduser -u 1000 -D -S -G croco croco \

# コンパイルに必要なライブラリのインストール
    && apk add --no-cache --virtual .persistent-deps \
        nginx \
        supervisor \
# PHPと各moduleのインストール
        php5 \
        php5-common \
        php5-cli \
        php5-curl \
        php5-fpm \
        php5-zlib \
        php5-pdo \
        php5-pdo_mysql \
        php5-gettext \
        php5-mcrypt \
        php5-phar \
        php5-posix \
        php5-ldap \
        php5-json \
        php5-opcache \
        php5-phalcon \

    && apk add --no-cache --virtual .build-deps \
        curl \
        autoconf \
        file \
        gcc \
        make \
        libc-dev \
        php5-dev \

# パッケージ化されていないmoduleのコンパイル
    && curl -fSL "http://php.net/get/php-5.6.23.tar.xz/from/this/mirror" -o "/tmp/php-5.6.23.tar.xz" \
    && tar -Jxf "/tmp/php-5.6.23.tar.xz" -C /tmp \
    && cd /tmp/php-5.6.23/ext/mbstring \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && echo "extension=mbstring.so" > /etc/php5/conf.d/mbstring.ini \

# amazon-elasticache-cluster-clientのインストール
    && curl -fSL "https://s3.amazonaws.com/elasticache-downloads/ClusterClient/PHP-5.6/latest-64bit" -o "/tmp/AmazonElastiCacheClusterClient.tar.gz" \
    && tar xzf /tmp/AmazonElastiCacheClusterClient.tar.gz -C /tmp \
    && cp /tmp/AmazonElastiCacheClusterClient-1.0.0/amazon-elasticache-cluster-client.so /usr/lib/php5/modules/memcached.so \
    && ln -s /usr/lib/libsasl2.so.3.0.0 /usr/lib/libsasl2.so.2 \
    && cp /tmp/AmazonElastiCacheClusterClient-1.0.0/memcached.ini /etc/php5/conf.d/memcached.ini \
    && echo "extension=memcached.so" >> /etc/php5/conf.d/memcached.ini \

# igbinaryのインストール
    && curl -fSL "https://github.com/igbinary/igbinary/archive/master.zip" -o "/tmp/igbinary.zip" \
    && unzip -q /tmp/igbinary.zip -d /tmp \
    && cd /tmp/igbinary-master \
    && phpize \
    && ./configure CFLAGS="-O2 -g" --enable-igbinary \
    && make \
    && make install \
    && echo "extension=igbinary.so" > /etc/php5/conf.d/igbinary.ini \

# コンパイルに使ったファイルの削除
    && rm -rf /tmp/* \
    && apk del .build-deps


#EXPOSE 9000
#CMD ["php-fpm"]
