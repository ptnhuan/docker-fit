# Use the official PHP 8 FPM image
FROM php:8.0-apache

# Declare composer version
ENV COMPOSER_VERSION=2.4.4

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        zip \
        unzip \
        git \
        supervisor \
        libpng-dev \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libzip-dev \
        gnupg \
        tzdata \
        autoconf \
        make \
        gcc \
        g++ \
        linux-headers-amd64 \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
        bcmath \
        gd \
        opcache \
        sockets \
        zip \
        mysqli \
        pdo_mysql \
        exif \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set timezone
ENV TZ=Asia/Tokyo
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf

# Install Composer with specified version
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=$COMPOSER_VERSION

# Create app user
RUN adduser --disabled-password --gecos '' --uid 1000 app
RUN a2enmod rewrite

# Copy start script and set execute permissions
COPY start /usr/local/bin/
RUN chmod +x /usr/local/bin/start

# Set the default command to run the start script
# CMD ["start"]
