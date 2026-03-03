FROM php:8.4-fpm

# 1. Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    curl \
    libzip-dev \
    libpng-dev \
    libxml2-dev \
    libonig-dev \
    libssl-dev \
    $PHPIZE_DEPS \
    && rm -rf /var/lib/apt/lists/*

# 2. Instalar extensiones nativas de PHP
RUN docker-php-ext-install pdo pdo_mysql xml ctype zip

# 3. Instalar extensión de MongoDB via PECL
RUN pecl install mongodb && docker-php-ext-enable mongodb

# 4. Se instala, y en el mismo paso se habilita
RUN pecl install xdebug || true && docker-php-ext-enable xdebug

# 5. Instalar Composer
RUN curl -sS https://getcomposer.org/installer | php \
    -- --install-dir=/usr/local/bin --filename=composer

# 6. Configuración PHP (copiar el ini de desarrollo)
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 7. Crear usuario y grupo (alineado con tu docker-compose)
RUN groupadd -g 1000 app && \
    useradd -u 1000 -g 1000 -m -s /bin/bash app

# Ajustar permisos
RUN chown -R app:app /var/www/html

# 8. Directorio de trabajo
WORKDIR /var/www/html