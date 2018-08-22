FROM php:7.2-fpm
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# change sources
COPY sources.list /etc/apt/sources.list

# Extensions: ctype, dom, fileinfo, ftp, hash, iconv, json, pdo, pdo_sqlite, session,
# tokenizer, simplexml, xml, xmlreader, xmlwriter and phar are bundled and compiled into
# PHP by default. If missing, install them directly by `docker-php-ext-install extension_name`

# Notice:
# 1. Mcrypt was DEPRECATED in PHP 7.1.0, and REMOVED in PHP 7.2.0.
# 2. opcache requires PHP version >= 7.0.0.
# 3. soap requires libxml2-dev.
# 4. xml, xmlrpc, wddx require libxml2-dev and libxslt-dev.
# 5. Line `&& :\` is just for better reading and do nothing.
RUN apt-get update \
    && apt-get install -y libfreetype6-dev \
      libjpeg62-turbo-dev \
      libpng-dev \
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
      iputils-ping telnet \
      git vim \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && :\
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) ldap \
    && :\
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl \
    && :\
    && apt-get install -y libxml2-dev \
    && apt-get install -y libxslt-dev \

    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install xmlrpc \
    && docker-php-ext-install wddx \
    && :\
    && apt-get install -y libbz2-dev \
    && docker-php-ext-install bz2 \
    && :\
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install calendar \
    && docker-php-ext-install sockets \
    && docker-php-ext-install opcache \
    #&& docker-php-ext-install gettext \
    #&& docker-php-ext-install shmop \
    #&& docker-php-ext-install sysvmsg \
    #&& docker-php-ext-install sysvsem \
    #&& docker-php-ext-install sysvshm \
    #&& docker-php-ext-install pdo_firebird \
    #&& docker-php-ext-install pdo_dblib \
    #&& docker-php-ext-install pdo_oci \
    #&& docker-php-ext-install pdo_odbc \
    #&& docker-php-ext-install pdo_pgsql \
    #&& docker-php-ext-install pgsql \
    #&& docker-php-ext-install oci8 \
    #&& docker-php-ext-install odbc \
    #&& docker-php-ext-install dba \
    #&& docker-php-ext-install interbase \
    #&& :\
    #&& apt-get install -y libmcrypt-dev \
    #&& docker-php-ext-install mcrypt \
    #&& :\
    #&& apt-get install -y curl \
    #&& apt-get install -y libcurl3 \
    #&& apt-get install -y libcurl4-openssl-dev \
    #&& docker-php-ext-install curl \
    #&& :\
    #&& apt-get install -y libreadline-dev \
    #&& docker-php-ext-install readline \
    #&& :\
    #&& apt-get install -y libsnmp-dev \
    #&& apt-get install -y snmp \
    #&& docker-php-ext-install snmp \
    #&& :\
    #&& apt-get install -y libpspell-dev \
    #&& apt-get install -y aspell-en \
    #&& docker-php-ext-install pspell \
    #&& :\
    #&& apt-get install -y librecode0 \
    #&& apt-get install -y librecode-dev \
    #&& docker-php-ext-install recode \
    #&& :\
    #&& apt-get install -y libtidy-dev \
    #&& docker-php-ext-install tidy \
    #&& :\
    #&& apt-get install -y libgmp-dev \
    #&& ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    #&& docker-php-ext-install gmp \
    #&& :\
    #&& apt-get install -y postgresql-client \
    #&& apt-get install -y mysql-client \
    #&& :\
    #&& apt-get install -y libc-client-dev \
    #&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    #&& docker-php-ext-install imap \
    #&& :\
    #&& apt-get install -y libldb-dev \
    #&& apt-get install -y libldap2-dev \
    #&& docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    #&& docker-php-ext-install ldap \
    && apt-get clean \
    && apt-get autoremove --purge -y

# Composer install
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# Install extension using pecl
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
    #&& pecl install mcrypt-1.0.1 \
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

# Write Permission
RUN usermod -u 1000 www-data