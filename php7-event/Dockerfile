FROM alpine:3.4

RUN set -xe \
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
        curl \
        g++ \
        gcc \
        git \
        gzip \
        libc-dev \
        libtool \
        libevent-dev \
        linux-headers \
        make \
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
        libevent \
        libstdc++ \
        openssl \
        pcre \
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

# eventのインストール
    && curl -fSL "https://pecl.php.net/get/event-2.1.0.tgz" -o "/tmp/event-2.1.0.tgz" \
    && tar xzvf /tmp/event-2.1.0.tgz -C /tmp \
    && cd /tmp/event-2.1.0 \
    && phpize7 \
    && ./configure --with-php-config=/usr/bin/php-config7 --without-event-openssl --with-event-core \
    && make \
    && make install \
    && echo "extension=event.so" > /etc/php7/conf.d/20_event.ini \

# コンパイルに使ったファイルの削除
    && rm -rf /tmp/* \
    && apk del .build-deps 
