FROM php:7.3-apache

RUN  a2enmod rewrite

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

RUN apt-get update && apt-get install -y \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libjpeg-dev \
  libpng-dev \
  libmcrypt-dev \
  libicu-dev \
  libpq-dev \
  libzip-dev \
  default-libmysqlclient-dev \
  ruby-full \
  nodejs \
  nano \
  cron \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
  && docker-php-ext-install pdo_mysql mysqli mbstring gd iconv intl mbstring mysqli pcntl pdo_mysql pdo_pgsql pgsql zip opcache \
  && apt-get clean

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# SASS compiler
RUN su -c "gem install sass"

# Timezone change Asia/Tkyo
RUN cp /usr/share/zoneinfo/Japan /etc/localtime