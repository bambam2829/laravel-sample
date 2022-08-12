FROM php:8.0-fpm-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]
#FROM php:7.2.10-fpm-alpine
COPY ./source /var/www/html

ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer
#RUN docker-php-ext-install pdo_mysql
COPY ./php-fpm/php.ini /usr/local/etc/php/php.ini

# timezone
ARG TZ=Asia/Tokyo

# composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer

#gitインストール
RUN apt-get update
RUN apt-get install -y git

WORKDIR /var/www/html

# composerインストール
RUN composer install

RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath && \
  chmod -R a+w storage/ bootstrap/cache


# RUN apt-get update && \
#   apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
#   apt-get clean && \
#   rm -rf /var/lib/apt/lists/* && \
#   docker-php-ext-install intl pdo_mysql zip bcmath && \
#   curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
#   composer global require hirak/prestissimo && \
#   composer install && \
#   chmod -R a+w storage/ bootstrap/cache