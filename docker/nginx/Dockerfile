FROM nginx:1.20-alpine

ENV TZ Asia/Tokyo

# nginx config file
COPY ./docker/nginx/*.conf /etc/nginx/conf.d/

RUN chmod 644 /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html
