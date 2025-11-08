FROM php:8.2-cli

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpq-dev \
    libzip-dev \
    zip \
    unzip \
    && docker-php-ext-install pdo pdo_pgsql zip

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy existing files (will be overridden by volume in dev, but needed for build)
COPY . .

# Install PHP dependencies
RUN composer install --no-scripts --no-autoloader

# Generate app key (safe in dev; will be regenerated on first run if missing)
RUN php artisan key:generate

# Expose port
EXPOSE 8000

# Keep container running
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]