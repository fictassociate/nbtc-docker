FROM php:7.1-apache

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

# Install useful tools
RUN apt-get -y install apt-utils nano wget dialog

# Install important libraries
RUN apt-get -y install --fix-missing apt-utils build-essential git curl libcurl4 libcurl3-dev zip openssl

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install xdebug
# RUN pecl install xdebug-2.6.0
# RUN docker-php-ext-enable xdebug

# utils
RUN apt-get update && \
        apt-get install -y apt-utils freetds-dev 

# zip, socket, mbstring
RUN docker-php-ext-install sockets

RUN docker-php-ext-configure pdo_dblib --with-libdir=lib/x86_64-linux-gnu && \
        docker-php-ext-install pdo_dblib

RUN docker-php-ext-configure calendar && docker-php-ext-install calendar

# Install redis
#RUN pecl install redis-4.0.1
#RUN docker-php-ext-enable redis

# Other PHP7 Extensions

#Redis
RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

#RUN apt-get -y install libsqlite3-dev libsqlite3-0 mysql-client
RUN apt-get install -y mariadb-client libmariadbclient-dev libsqlite3-dev libsqlite3-0
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install mysqli

RUN docker-php-ext-install curl
RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json

RUN apt-get -y install zlib1g-dev
RUN docker-php-ext-install zip

RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

RUN docker-php-ext-install mbstring
RUN docker-php-ext-install gettext

RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

RUN apt-get update && apt-get install -y sendmail

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Uncomment these two lines to fix "non-UTF8" chars encoding and time format problems
ADD freetds.conf /etc/freetds/
ADD locales.conf /etc/freetds/
ADD 000-default.conf /etc/apache2/sites-enable/
ADD php.ini /usr/local/etc/php/php.ini

# Enable apache modules
RUN a2enmod rewrite headers
