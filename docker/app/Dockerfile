FROM php:8.2.8-fpm-bookworm

LABEL Maintainer="Henri Azevedo <hnr.azevedo@gmail.com>"
LABEL Version="0.1"

ARG ext_bcmath=true
ARG ext_ctype=true
ARG ext_fileinfo=false
ARG ext_mbstring=true
ARG ext_openssl=true
ARG ext_pdo_mysql=true
ARG ext_xml=true
ARG ext_xdebug=false
ARG ext_compression=true
ARG ext_gd=true
ARG ext_opcache=true
ARG ext_mcrypt=true
ARG composer=false

# Additional tools
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
     curl \
     openssl \
     libssl-dev

# Common php-ext and requirements
RUN apt-get install -y --no-install-recommends libpq-dev libz-dev \
  && docker-php-ext-install pcntl \
  && docker-php-ext-install session \
  && docker-php-ext-install phar \
  && docker-php-ext-install iconv \
  && docker-php-ext-install pdo

RUN if [ ${ext_bcmath} = true ]; then \
    docker-php-ext-install bcmath \
;fi

RUN if [ ${ext_ctype} = true ]; then \
    docker-php-ext-install ctype \
;fi

RUN if [ ${ext_fileinfo} = true ]; then \
    docker-php-ext-install fileinfo \
;fi

RUN if [ ${ext_mbstring} = true ]; then \
    apt-get install libonig-dev && \
    docker-php-ext-install mbstring \
;fi

RUN if [ ${ext_pdo_mysql} = true ]; then \
    docker-php-ext-install pdo_mysql \
;fi

RUN if [ ${ext_xml} = true ]; then \
    apt-get install -y --no-install-recommends libxml2-dev \
 && docker-php-ext-install xml \
;fi

RUN if [ ${ext_xdebug} = true ]; then \
    pecl install xdebug \
    docker-php-ext-enable xdebug \
;fi

RUN if [ ${ext_compression} = true ]; then \
    apt-get install -y --no-install-recommends libbz2-dev zlib1g-dev libzip-dev \
 && docker-php-ext-install zip \
    bz2 \
;fi

RUN if [ ${ext_gd} = true ]; then \
    apt-get install -y --no-install-recommends libjpeg62-turbo-dev libpng-dev libfreetype6-dev libwebp-dev \
&& docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
 && docker-php-ext-install gd \
;fi

RUN if [ ${ext_opcache} = true ]; then \
    docker-php-ext-install opcache \
;fi

RUN if [ ${ext_mcrypt} = true ]; then \
    apt-get install -y --no-install-recommends libmcrypt-dev && \
 pecl install mcrypt-1.0.6 \
 &&  docker-php-ext-enable mcrypt \
;fi

RUN if [ ${composer} = true ]; then \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
;fi

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Copy desk configration
COPY ./php.ini /usr/local/etc/php/conf.d/desk.ini

# Clear package lists
RUN rm -rf /var/lib/apt/lists/*

# Permissions
RUN chown -R root:www-data /var/www/html
RUN chmod u+rwx,g+rx,o+rx /var/www/html
RUN find /var/www/html -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www/html -type f -exec chmod u+rw,g+rw,o+r {} +

CMD ["php-fpm"]

EXPOSE 9000
