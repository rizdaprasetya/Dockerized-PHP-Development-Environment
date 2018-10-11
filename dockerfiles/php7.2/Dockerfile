FROM php:7.2-apache

RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN apt-get update -y && apt-get install -y sendmail libpng-dev
RUN apt-get update && \
    apt-get install -y \
        zlib1g-dev 
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip

RUN apt-get update && apt-get install -y libpng-dev 
RUN apt-get install -y libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev \
  libfreetype6-dev
RUN docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
  --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir \
  --enable-gd-native-ttf
RUN docker-php-ext-install gd

RUN apt-get install -y libicu-dev
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl

# RUN apt-get install -y libmcrypt-dev
# RUN docker-php-ext-install mcrypt

RUN a2enmod rewrite