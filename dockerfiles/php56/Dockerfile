FROM php:5.6-fpm
MAINTAINER duzhenxun<5552123@qq.com>

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# change sources
COPY sources.list /etc/apt/sources.list

# Extensions: ctype, dom, fileinfo, ftp, hash, iconv, json, pdo, pdo_sqlite, session,
# tokenizer, simplexml, xml, xmlreader, xmlwriter and phar are bundled and compiled into
# PHP by default. If missing, install them directly by `docker-php-ext-install extension_name`
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
      libicu-dev \
      libxml2-dev \
      libxslt-dev \
      libbz2-dev \
      git vim \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) gd ldap intl soap xsl xmlrpc bz2 wddx zip pcntl pdo_mysql mysqli mbstring exif bcmath calendar sockets mcrypt \
    && apt-get clean \
    && apt-get autoremove --purge -y

# Composer install
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# Write Permission
RUN usermod -u 1000 www-data