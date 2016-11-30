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
        alpine-sdk \
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

# abuild checksum && abuild -r
    && adduser -D yujiro \
    && addgroup yujiro abuild \
    && chown -R yujiro /usr/lib/php7 \
    && chown -R yujiro /usr/include/php7 \
    && echo "yujiro  ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && mkdir -p /var/cache/distfiles \
    && chgrp abuild /var/cache/distfiles \
    && chmod g+w /var/cache/distfiles \
    && echo "PACKAGER='yujiro <$useremail>'" >> /etc/abuild.conf \
    && su yujiro \
    && cd /home/yujiro \
    && git config --global user.name 'yujiro' \
    && git config --global user.email 'yujiro@cro-co.co.jp' 
