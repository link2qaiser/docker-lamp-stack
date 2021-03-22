ARG APACHE_VERSION=""
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine

RUN apk update; \
    apk upgrade;

# Copy apache vhost file to proxy php requests to php-fpm container
COPY demo.apache.conf /usr/local/apache2/conf/demo.apache.conf
RUN echo "Include /usr/local/apache2/conf/demo.apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf

FROM php:7.3-apache
RUN a2enmod rewrite
RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN docker-php-ext-install exif && docker-php-ext-enable exif

#############Couchbase###########################
# php extension installer for the common ones
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd imagick imap mailparse mysqli opcache xdebug zip

# Get the Couchbase Repository Key
ADD https://packages.couchbase.com/clients/c/repos/deb/couchbase.key .

RUN apt-get update \
    && apt-get install -y lsb-release software-properties-common gnupg2 build-essential zlib1g-dev \
    && apt-key add ./couchbase.key \
    && rm ./couchbase.key \
    && apt-add-repository 'deb https://packages.couchbase.com/clients/c/repos/deb/debian10 buster buster/main' \
    && apt-get update \
    && apt-get install -y libcouchbase-dev libcouchbase3-libevent libcouchbase3-tools

# Install ext-couchbase
# After the fix is released in 3.0.3
RUN pecl install https://packages.couchbase.com/clients/php/couchbase-3.0.3.tgz
# Until that fix
#RUN pecl install https://packages.couchbase.com/clients/php/couchbase-3.0.3.tgz

WORKDIR /var/www/html

# Copy configuration files
COPY ./config/php.ini /usr/local/etc/php/php.ini