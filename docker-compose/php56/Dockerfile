FROM php:5.6-fpm
MAINTAINER duzhenxun<5552123@qq.com>
# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# change sources
COPY sources.list /etc/apt/sources.list

RUN apt-get update \
    && apt-get install -y libfreetype6-dev \
      libjpeg62-turbo-dev \
      libpng-dev \
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
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) gd ldap intl soap xsl xmlrpc bz2 wddx zip pcntl pdo_mysql mysqli exif bcmath calendar sockets  \
    && apt-get clean \
    && apt-get autoremove --purge -y

# Write Permission
RUN usermod -u 1000 www-data