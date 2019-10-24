##
## Name: laravel-test-pokeapi
## Author: https://about.me/henrycv
##

FROM php:7.3-fpm

# Copy composer.lock and composer.json
# COPY composer.lock composer.json /var/www/

# Root directory path
ARG ROOT_DIR=/usr/share/nginx/html

# Set working directory
WORKDIR $ROOT_DIR

ENV DEBIAN_FRONTEND noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    --no-install-recommends apt-utils \
    build-essential \
    default-mysql-client \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-configure zip --with-libzip 
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install mysqli 
# RUN docker-php-ext-enable mysqli

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Install Laravel
RUN composer global require laravel/installer

# # Generate project
# RUN composer create-project --prefer-dist laravel/laravel laravel-test-i

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]