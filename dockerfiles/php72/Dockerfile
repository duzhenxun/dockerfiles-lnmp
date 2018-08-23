FROM php:7.2-fpm
MAINTAINER duzhenxun<5552123@qq.com>
# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# change sources
COPY sources.list /etc/apt/sources.list
RUN apt-get update \
    && apt-get install -y libfreetype6-dev \
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
      libldb-dev \
      libldap2-dev \
      libssl-dev \
      libmosquitto-dev \
      librabbitmq-dev \
      libicu-dev \
      libxml2-dev \
      libxslt-dev \
      libbz2-dev \
      git vim \
    && :\
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && :\
    && docker-php-ext-install -j$(nproc) gd ldap intl soap xsl xmlrpc wddx bz2 zip pcntl pdo_mysql mysqli mbstring calendar sockets opcache exif bcmath \
    && :\
    && apt-get clean \
    && apt-get autoremove --purge -y

# Composer install
COPY ./ext/composer.phar /usr/local/bin/composer
#RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com
# 使用源码包方式安装扩展
# Install Redis extension from source
COPY ./ext/redis-4.1.0.tgz /tmp/redis.tgz
RUN ls /tmp
RUN mkdir -p /tmp/redis \
    && tar -xf /tmp/redis.tgz -C /tmp/redis --strip-components=1 \
    && rm /tmp/redis.tgz \
    && docker-php-ext-configure /tmp/redis --enable-redis \
    && docker-php-ext-install /tmp/redis \
    && rm -r /tmp/redis

# zendopcache
#COPY ./ext/zendopcache-7.0.5.tgz /tmp/
#RUN cd /tmp/ \
#&& tar -xf zendopcache-7.0.5.tgz \
#&& rm zendopcache-7.0.5.tgz \
#&& ( cd zendopcache-7.0.5 && phpize && ./configure && make && make install ) \
#&& rm -r zendopcache-7.0.5 \
#&& docker-php-ext-enable opcache


# libmemcached
#COPY ./ext/libmemcached-1.0.18.tar.gz /tmp/
#RUN cd /tmp/ \
#    && tar -xf libmemcached-1.0.18.tar.gz \
#    && rm libmemcached-1.0.18.tar.gz \
#    && cd libmemcached-1.0.18 \
#    && ./configure --prefix=/usr/local/libmemcached --with-memcached \
#    && make && make install \
#    && rm -rf /tmp/libmemcached-1.0.18

# memcached
COPY ./ext/memcached-3.0.4.tgz /tmp/
RUN cd /tmp/ \
    && tar -xf memcached-3.0.4.tgz \
    && cd memcached-3.0.4 \
    && phpize \
    && ./configure -enable-memcached -with-php-config=/usr/local/bin/php-config \
    && make && make install \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini \
    && rm -rf /tmp/memcached-3.0.4

# 使用git方式
#Swoole
RUN cd /tmp \
    && git clone https://gitee.com/swoole/swoole.git \
    && cd swoole \
    && phpize \
    && ./configure --enable-openssl -with-php-config=/usr/local/bin/php-config \
    && make \
    && make install \
    && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini \
    && rm -rf /tmp/swoole

# pecl 安装
# Notice: if pecl install get error
#    `No releases available for package "pecl.php.net/xxx"`
# or
#    `Package "xxx" does not have REST xml available`
# Please turn on proxy (The proxy IP may be docker host IP or others):

#RUN pear config-set http_proxy http://10.0.75.1:1080
#RUN pecl install redis-3.1.4 \
    #&& docker-php-ext-enable redis \
    #&& pecl install xdebug-2.4.1 \
    #&& docker-php-ext-enable xdebug \
    #&& apt-get install -y libmagickwand-dev \
    #&& pecl install imagick-3.4.3 \
    #&& docker-php-ext-enable imagick \
    #&& apt-get install -y libmemcached-dev zlib1g-dev \
    #&& pecl install memcached-2.2.0 \
    #&& docker-php-ext-enable memcached
    #&& pecl install gmagick-2.0.5RC1 \
    #&& pecl install imagick-3.4.3 \
    #&& pecl install memcached-3.0.4 \
    #&& pecl install redis-4.0.2 \
    #&& pecl install mongodb-1.4.3 \
    #&& pecl install swoole-2.1.3 \
    #&& pecl install ssh2-1.1.2 \
    #&& pecl install yaf-3.0.7 \
    #&& pecl install yaconf-1.0.7 \
    #&& pecl install zip-1.15.2 \
    #&& pecl install zookeeper-0.5.0 \
    #&& pecl install Mosquitto-0.4.0 \
    #&& pecl install amqp-1.9.3 \
    #&& pecl install xdebug-2.6.0 \
    #&& docker-php-ext-enable gmagick memcached redis mcrypt mongodb swoole ssh2 yaf yaconf zip zookeeper mosquitto amqp \

# Write Permission
RUN usermod -u 1000 www-data