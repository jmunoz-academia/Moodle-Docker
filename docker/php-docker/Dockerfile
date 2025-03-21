FROM php:8.3-apache

# Instalar dependencias del sistema necesarias para las extensiones
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libonig-dev \
    libzip-dev \
    libicu-dev \
    && rm -rf /var/lib/apt/lists/*

# Configurar e instalar las extensiones necesarias para Moodle, incluyendo exif
RUN docker-php-ext-configure gd --with-jpeg --with-freetype \
    && docker-php-ext-install -j$(nproc) \
        mysqli \
        pdo_mysql \
        gd \
        xml \
        mbstring \
        curl \
        zip \
        intl \
        soap \
        opcache \
        exif

# Habilitar la extensión MySQLi (opcional)
RUN docker-php-ext-enable mysqli

# Copiar la configuración de OPcache (si tienes opcache.ini)
COPY opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Copiar la configuración personalizada de PHP
COPY php-custom.ini /usr/local/etc/php/conf.d/php-custom.ini

# Habilitar el módulo rewrite de Apache
RUN a2enmod rewrite

# Ajustar permisos del directorio web para el usuario www-data
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html
