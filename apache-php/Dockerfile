FROM php:8.1.0-apache

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

ENV APACHE_DOCUMENT_ROOT /var/www/mywebsite

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN apt-get update \
&& apt-get install -y vim libpng-dev libfreetype6-dev libjpeg62-turbo-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
&& docker-php-ext-install gd 

RUN docker-php-ext-install mysqli pdo pdo_mysql  \
&& pecl install xdebug-3.1.2 \
&& docker-php-ext-enable xdebug \
&& printf '[xdebug]\nxdebug.mode=develop,debug,profile,trace\nxdebug.start_with_request=yes\nxdebug.discover_client_host=true\nxdebug.client_host=host.docker.internal\nxdebug.client_port=9001\n\nextension=php_gd.so' >> /usr/local/etc/php/php.ini \
&& a2enmod rewrite
