# gd

The gd library can be used using the last part of the following **Dockerfile**:

    FROM php:7.4-apache

    WORKDIR /var/www/html/

    RUN echo 'set -e' > /boot.sh # this is the script which will run on boot

    COPY . .

    CMD sh /boot.sh && . /etc/apache2/envvars && apache2 -DFOREGROUND

    RUN apt-get update && \

    apt-get install -y libfreetype6-dev libjpeg62-turbo-dev && \
    docker-php-ext-configure gd --with-freetype --with-jpeg &&  \
    docker-php-ext-install gd