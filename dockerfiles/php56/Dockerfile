FROM php:5.6-fpm
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# change sources
COPY sources.list /etc/apt/sources.list


RUN apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libmemcached-dev \
        graphicsmagick \
        libgraphicsmagick1-dev \
        imagemagick \
        libmagickwand-dev \
        libssh2-1-dev \
        libzip-dev \
        libzookeeper-mt-dev \
        libldap2-dev \
        libssl-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) bcmath gd gettext pdo_mysql mysqli ldap pcntl soap sockets sysvsem xmlrpc mcrypt \
    && pecl install gmagick-1.1.7RC3 \
    && pecl install imagick-3.4.3 \
    && pecl install memcached-2.2.0 \
    && pecl install redis-4.0.2 \
    && pecl install mongodb-1.4.3 \
    && pecl install swoole-1.10.5 \
    && pecl install ssh2-0.13 \
    && pecl install zip-1.15.2 \
    && pecl install zookeeper-0.5.0 \
    && docker-php-ext-enable gmagick memcached redis mongodb swoole ssh2 zip zookeeper \
    && apt-get clean \
    && apt-get autoremove --purge -y

# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH


# Write Permission
RUN usermod -u 1000 www-data
