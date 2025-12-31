FROM nginx:alpine

# bashをインストール
RUN apk add --no-cache bash

# デフォルトの設定を削除
RUN rm /etc/nginx/conf.d/default.conf

# nginxの設定ファイルを配置
COPY docker/dev/nginx/nginx.conf /etc/nginx/nginx.conf
COPY docker/dev/nginx/dev-k8s.tai333.cloud.conf /etc/nginx/conf.d/dev-k8s.tai333.cloud.conf

WORKDIR /var/www/dev-k8s.tai333.cloud