FROM php:5-apache

RUN apt-get update
RUN apt-get install -y g++ 

RUN apt-get install -y libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

RUN apt-get install -y libxml2-dev
RUN apt-get install -y wget
RUN docker-php-ext-install -j$(nproc) soap

RUN apt-get install -y libmcrypt-dev
RUN apt-get install -y zip
RUN docker-php-ext-install -j$(nproc) mcrypt

RUN docker-php-ext-install -j$(nproc) pdo pdo_mysql

RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng12-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install -j$(nproc) gettext mbstring
RUN docker-php-ext-install -j$(nproc) mcrypt

RUN pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN a2enmod rewrite

RUN apt-get autoremove -y && apt-get clean all

COPY etc/php.ini /usr/local/etc/php/

RUN wget https://files.magerun.net/n98-magerun.phar

RUN chmod +x ./n98-magerun.phar

RUN cp ./n98-magerun.phar /usr/local/bin/
COPY etc/ssl_vhost.conf /etc/apache2/sites-available/
COPY etc/crt /etc/apache2/crt/

RUN a2enmod ssl
RUN a2ensite ssl_vhost.conf
#RUN service apache2 restart