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
        curl \
        gcc \
        git \
        gzip \
        linux-headers \
        make \
        openldap-dev \
        openssl-dev \
        pcre-dev \
        tar \
        zlib-dev \

# 恒久的なパッケージのインストール
    && apk --update add --no-cache \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
           --virtual .persistent-deps \

# サービス・ライブラリのインストール
        libldap \
        openssl \
        pcre \
        zlib \

# Install Nginx from source, see http://nginx.org/en/linux_packages.html#mainline
    && curl -fSL "https://github.com/nginx/nginx/archive/release-1.11.5.tar.gz" -o "/tmp/nginx-1.11.5.tgz" \
    && tar xzvf /tmp/nginx-1.11.5.tgz -C /tmp \
    && git clone https://github.com/kvspb/nginx-auth-ldap.git /tmp/nginx-auth-ldap \
    && cd /tmp/nginx-release-1.11.5 \
    && ./auto/configure \
        --prefix=/usr/share/nginx \
        --sbin-path=/usr/sbin/nginx \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --pid-path=/var/run/nginx.pid \
        --lock-path=/var/run/nginx.lock \
        --user=croco \
        --group=croco \
        --with-http_auth_request_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_random_index_module \
        --with-http_realip_module \
        --with-http_secure_link_module \
        --with-http_slice_module \
        --with-http_stub_status_module \
        --with-http_sub_module \
        --with-http_v2_module \
        --with-file-aio \
        --with-stream \
        --add-module=/tmp/nginx-auth-ldap \
    && make \
    && make install \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \

# コンパイルに使ったファイルの削除
    && rm -rf /tmp/* \
    && apk del .build-deps 

# ファイルの追加
ADD opt /opt

EXPOSE 80
CMD ["nginx", "-c", "/opt/conf/nginx.conf", "-g", "daemon off;"]
