FROM php:7.4-fpm

ARG APP_ID=1000
RUN groupadd -g "$APP_ID" app \
  && useradd -g "$APP_ID" -u "$APP_ID" -d /var/www -s /bin/bash app

RUN mkdir -p /etc/nginx/html /var/www/symfony /sock \
  && chown -R app:app /etc/nginx /var/www/symfony /usr/local/etc/php/conf.d /sock

RUN apt-get update -yqq \
    && apt-get install -yqq \
    ca-certificates \
    lsb-release \
    sudo \
    apt-transport-https \
    mariadb-client \
    iputils-ping \
    vim \
    zlib1g-dev \
    libzip-dev \
    libxslt-dev \
    libpng-dev \
    libmcrypt-dev \
    libreadline-dev \
    libonig-dev \
    procps \
    git \
    gcc \
    make \
    wget \
    mc \
    curl \
    cron \
    zip \
    xvfb \
    libfontconfig1 \
    gnupg \
    acl \
    libxrender1 \
    libxml2-dev \
    libmagickwand-dev

RUN sed -i "s|DEFAULT@SECLEVEL=2|DEFAULT@SECLEVEL=1|g" /etc/ssl/openssl.cnf

RUN pecl install mcrypt-1.0.3
RUN docker-php-ext-configure gd --with-jpeg --with-freetype
RUN docker-php-ext-install \
    pdo pdo_mysql zip xsl gd intl opcache exif mbstring xml soap
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && php composer-setup.php --2.2 && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer

USER app:app

WORKDIR /var/www/symfony
