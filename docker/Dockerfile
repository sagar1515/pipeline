# Use official PHP image as base
FROM php:8.2-fpm

# Set working directory
WORKDIR /var/www/html

# Install system dependencies
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    curl \
    git \
    nano \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions required for Laravel, including 'zip'
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy application code
COPY . /var/www/html

# Install Laravel dependencies
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Ensure necessary directories exist
RUN mkdir -p /var/www/html/storage /var/www/html/bootstrap/cache

# Set permissions for Laravel's storage and cache directories
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache \
    && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# Set the default user to www-data
USER www-data

# Expose the port used by PHP-FPM
EXPOSE 9000

# Start PHP-FPM server
CMD ["php-fpm"]

