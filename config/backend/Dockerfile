FROM php:7.4-apache

# Install system dependencies
RUN apt-get update \
    && apt-get install -y \
        curl \
        zip \
        unzip \
        git \
        supervisor \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        gnupg \
        tzdata \
    && docker-php-ext-install \
        bcmath \
        gd \
        opcache \
        sockets \
        zip \
        mysqli \
        pdo_mysql

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug


# Set timezone
ENV TZ=Asia/Tokyo
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf

# Create app user
RUN adduser --disabled-password --gecos '' --uid 1000 app
RUN a2enmod rewrite

# Copy start script and set execute permissions
COPY start /usr/local/bin/
RUN chmod +x /usr/local/bin/start

# Set the default command to run the start script
# CMD ["start"]
