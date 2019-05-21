FROM php:7.3-apache

ADD custom-vhost.conf /etc/apache2/sites-available/000-default.conf

# Install Xdebug
RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.7.1.tgz' -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && ( \
    cd xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xdebug \
    && docker-php-ext-enable xdebug

    COPY ./php.ini /usr/local/etc/php/php.ini
    ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE = DontWarn

RUN curl -sl https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get update && apt-get install -y \
    libicu-dev \
    libpq-dev \
    libzip-dev \
    libpng-dev \
    mysql-client \
    default-libmysqlclient-dev \
    ruby-full \
    nodejs \
    && rm -r /var/lib/apt/lists/* \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-install \
    intl \
    mbstring \
    mysqli \
    pcntl \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    zip \
    opcache \
    gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# SASS compiler
RUN su -c "gem install sass"