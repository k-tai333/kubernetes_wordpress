FROM php:8.2-fpm-alpine

RUN apk add --no-cache $PHPIZE_DEPS \
    bash \
    git \
    libzip-dev \
    oniguruma-dev \
    icu-dev \
    zlib-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    libwebp-dev \
    freetype-dev

# GD拡張の設定
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp

# PHP拡張のインストール
RUN docker-php-ext-install mysqli \
    pdo_mysql \
    mbstring \
    zip \
    intl \
    gd

# Redis拡張のインストール（pecl）
RUN pecl install redis

# Redis拡張を有効化
RUN docker-php-ext-enable redis

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# ホストとコンテナのユーザーID・グループIDを合わせる
ARG PUID=1000
ARG PGID=1000
RUN addgroup -g ${PGID} app && adduser -D -G app -u ${PUID} app

WORKDIR /var/www/dev-k8.tai333.cloud

# WordPress のソースコードをコンテナにコピー（マスター置き場）
COPY --chown=app:app ./wordpress/ /usr/src/wordpress/

# 設定ファイルを配置
COPY ./docker/dev/app/www.conf /usr/local/etc/php-fpm.d/www.conf

# デフォルトの zz-docker.conf が listen=9000 で上書きするので削除
RUN rm -f /usr/local/etc/php-fpm.d/zz-docker.conf

# nginx 用のグループを作成（nginx コンテナとグループIDを合わせるため）
RUN addgroup -g 101 nginx || true
