FROM php:7.3-fpm

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        procps \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        zlib1g-dev libicu-dev g++ \

    && docker-php-ext-install -j$(nproc) bcmath iconv mbstring mysqli pdo_mysql zip intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install sockets

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

CMD ["php-fpm"]

RUN apk add --no-cache supervisor
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /etc/supervisor.d
COPY supervisor/supervisord.conf /etc/
COPY supervisor/supervisor.d/ /etc/supervisor.d/

CMD ["/usr/bin/supervisord"]
