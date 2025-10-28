FROM php:8.2-apache

# Paquets utiles + extensions PHP fréquentes pour Drupal
RUN apt-get update && apt-get install -y \
    git unzip libpng-dev libjpeg62-turbo-dev libicu-dev libzip-dev libonig-dev zlib1g-dev \
 && docker-php-ext-install gd intl opcache zip \
 && rm -rf /var/lib/apt/lists/*

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Config Apache: Drupal docroot = /var/www/html/web
ENV APACHE_DOCUMENT_ROOT=/var/www/html/web
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf \
 && sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT%/web}/!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf \
 && a2enmod rewrite

WORKDIR /var/www/html

# Copie fichiers Composer d'abord (cache Docker)
COPY drupal/composer.json drupal/composer.lock ./ 
# Si le projet utilise des extras (ex: patches), on copie tout et on installera
COPY drupal/ ./

# Install des dépendances PHP/Drupal (prod)
RUN COMPOSER_ALLOW_SUPERUSER=1 composer install --no-dev --prefer-dist --no-interaction --optimize-autoloader

# Droits basiques (adapter si besoin)
RUN chown -R www-data:www-data /var/www/html
