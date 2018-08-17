FROM php:5.6-fpm
MAINTAINER duzhenxun "5552123@qq.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        vim \
        curl \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libssl-dev \
        && docker-php-ext-install pdo_mysql mysqli pcntl soap opcache \
        && docker-php-ext-enable pdo_mysql \
        && docker-php-ext-enable mysqli \
        && docker-php-ext-enable pcntl \
        && docker-php-ext-enable opcache \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-enable gd \
        && pecl install imagick-3.4.3 \
        && docker-php-ext-enable imagick \
        && pecl install redis-2.2.8 \
        && docker-php-ext-enable redis \
        && pecl install libevent-0.1.0 \
        && docker-php-ext-enable libevent \
        && echo no | pecl install memcached-3.0.4 \
        && docker-php-ext-enable memcached \
        && pecl install Xdebug-2.5.0 \
        && docker-php-ext-enable xdebug \
        && pecl install swoole-2.1.0 \
        && docker-php-ext-enable swoole \
        && export TERM=xterm \
        && apt-get clean \
        && apt-get autoclean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH


WORKDIR /data

# 写权限
RUN usermod -u 1000 www-data
