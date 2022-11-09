FROM php:7.4-apache

COPY ./php/php.ini /usr/local/etc/php/
COPY ./apache/*.conf /etc/apache2/sites-enabled/

RUN apt-get update \
  && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev libwebp-dev \
  && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-webp-dir=/usr/include/ \
  && docker-php-ext-install pdo_mysql mysqli mbstring gd iconv \
  && apt-get -y install libzip-dev \
  && apt-get install -y zlib1g-dev \
  && docker-php-ext-install zip

  # && apt-get -y install libzip-dev \
  # && apt-get install -y zlib1g-dev \  
  # && pdo_mysql mysqli mbstring gd iconv \
  # && apt-get install gcc \
  # && apt-get install imagemagick \

RUN apt-get update \
  && apt-get install -y libmagickwand-dev  \
  && pecl install imagick \
  && docker-php-ext-enable imagick

# RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
# RUN docker-php-ext-install -j$(nproc) gd


# mod_rewriteを 有効に
RUN /bin/sh -c a2enmod rewrite
RUN /bin/sh -c a2enmod include
# RUN echo "AddType application/x-httpd-php .html" >> /etc/apache2/apache2.conf
RUN echo "AddHandler application/x-httpd-php .php .html" >> /etc/apache2/apache2.conf

RUN a2enmod rewrite

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN composer global require "laravel/installer"
