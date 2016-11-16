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
        zlib 
