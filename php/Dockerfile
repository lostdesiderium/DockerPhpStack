FROM php:8.2-fpm
RUN apt-get update && apt-get install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip \
		libfreetype-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \ 
    && docker-php-ext-install zip

WORKDIR /var/www/project/

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash

# Export symfony cli path to shell configuration file for easier calling symfony script
# Path may differ depending on symfony version
# If symfony command is not found in container then run same command inside container mv /root/.symfony5/bin/symfony
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony