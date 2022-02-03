FROM php:7.4.3-apache
LABEL maintainer="YOK"

RUN docker-php-ext-install mysqli pdo pdo_mysql

COPY lamp/html/. /var/www/html