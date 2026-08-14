# ベースイメージの指定
FROM node:22-bookworm-slim as node
FROM php:8.4-fpm-bookworm

# Viteのために5173ポートを開放（nodeが不要であればコメントアウト）
EXPOSE 5173

# コンテナ内のサーバー環境変数を設定
ENV TZ=Asia/Tokyo \
    # locale
    LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8

# パッケージのインストール
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    git \
    unzip \
    vim \
    libzip-dev \
    libicu-dev \
    libonig-dev \
    # パッケージインストールで使用したキャッシュファイル削除
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    # PHPの拡張ライブラリインストール
    && docker-php-ext-install intl pdo_mysql zip bcmath \
    # xdebug install
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

# composer command
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
# node command
COPY --from=node /usr/local/bin /usr/local/bin
# npm command
COPY --from=node /usr/local/lib /usr/local/lib
# yarn command
COPY --from=node /opt /opt

# composer environment
ENV COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_HOME=/composer

# 作成した設定ファイルをコンテナ内にコピー
COPY ./docker/php/php.ini /usr/local/etc/php/php.ini
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# コンテナ内で作業する際のカレントディレクトリの指定
WORKDIR /var/www/html

# コンテナ起動時に実行するコマンド
# npm installはpackage.jsonが無くても`;`でエラーを無視して後続のphp-fpm起動を継続する
ENTRYPOINT [ "bash", "-c", "npm install; exec php-fpm" ]
