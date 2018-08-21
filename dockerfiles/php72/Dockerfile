FROM php:7.2-fpm
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# change sources
COPY sources.list /etc/apt/sources.list

#操作指令
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
        libmosquitto-dev \
        librabbitmq-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) bcmath gd gettext pdo_mysql mysqli ldap pcntl soap sockets sysvsem xmlrpc \
    && pecl install gmagick-2.0.5RC1 \
    && pecl install imagick-3.4.3 \
    && pecl install memcached-3.0.4 \
    && pecl install redis-4.0.2 \
    && pecl install mcrypt-1.0.1 \
    && pecl install mongodb-1.4.3 \
    && pecl install swoole-2.1.3 \
    && pecl install ssh2-1.1.2 \
    && pecl install yaf-3.0.7 \
    && pecl install yaconf-1.0.7 \
    && pecl install zip-1.15.2 \
    && pecl install zookeeper-0.5.0 \
    && pecl install Mosquitto-0.4.0 \
    && pecl install amqp-1.9.3 \
    && pecl install xdebug-2.6.0 \
    && docker-php-ext-enable gmagick memcached redis mcrypt mongodb swoole ssh2 yaf yaconf zip zookeeper mosquitto amqp \
    && apt-get clean \
    && apt-get autoremove --purge -y
# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH


# Write Permission
RUN usermod -u 1000 www-data