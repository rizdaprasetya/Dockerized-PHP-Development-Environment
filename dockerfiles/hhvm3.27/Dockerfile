FROM webdevops/hhvm-apache:ubuntu-16.04

RUN apt-get update -y && apt-get install -y sendmail libpng-dev
RUN apt-get update && \
    apt-get install -y \
        zlib1g-dev 

RUN apt-get update && apt-get install -y libpng-dev 
RUN apt-get install -y libwebp-dev libpng-dev libxpm-dev \
  libfreetype6-dev

RUN apt-get install -y libicu-dev

RUN a2enmod rewrite