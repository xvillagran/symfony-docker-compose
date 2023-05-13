FROM nginx:1.18

ARG APP_ID=1000
ARG SYMFONY_HOST_NAME=symfony.local.com

RUN groupadd -g "$APP_ID" app \
  && useradd -g "$APP_ID" -u "$APP_ID" -d /var/www/symfony -s /bin/bash app

RUN touch /var/run/nginx.pid

COPY ./default.conf /etc/nginx/conf.d/default.conf

RUN mkdir -p /etc/nginx/html /var/www/symfony \
  && chown -R app:app /etc/nginx /var/www /var/cache/nginx /var/log/nginx/ /var/run/nginx.pid

RUN apt-get install -y openssl;

RUN mkdir /etc/nginx/certs
RUN openssl req -x509 -nodes -days 365 -subj "/C=CA/ST=QC/O=Company, Inc./CN=$SYMFONY_HOST_NAME" \
    -addext "subjectAltName=DNS:$SYMFONY_HOST_NAME" -newkey rsa:2048 -keyout /etc/nginx/certs/symfony.key \
    -out /etc/nginx/certs/symfony.crt;

RUN chmod 664 /etc/nginx/certs/symfony.key
RUN chmod 664 /etc/nginx/certs/symfony.crt

EXPOSE 8443

USER app:app

VOLUME /var/www/symfony

WORKDIR /var/www/symfony