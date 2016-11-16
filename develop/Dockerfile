FROM alpine:3.4

RUN set -xe \
# �W��C���C�u�����̃C���X�g�[��
    && apk --update add \
           --virtual .build-deps \
        musl \
        musl-dev \

# �e��c�[���E�R���p�C���p���C�u�����̃C���X�g�[��
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

# �P�v�I�ȃp�b�P�[�W�̃C���X�g�[��
    && apk --update add --no-cache \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ \
           --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
           --virtual .persistent-deps \

# �T�[�r�X�E���C�u�����̃C���X�g�[��
        libldap \
        libstdc++ \
        openssl \
        pcre \
        supervisor \
        zlib 