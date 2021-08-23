FROM php:5.6-fpm
#因为官方镜像是用的debain stretch 所以第一步先替换源 如果是Jessie 要替换对应的源
RUN echo "deb http://mirrors.aliyun.com/debian stretch main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian stretch main contrib non-free" >> /etc/apt/sources.list  && \
    echo "deb http://mirrors.aliyun.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.lis

# 安装扩展
RUN apt-get update \
    && apt-get install -y --no-install-recommends --assume-yes git zip unzip vim wget curl \
       libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmemcached-dev zlib1g-dev libmagickwand-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd sockets zip pdo_mysql\
    && pecl update-channels \
    && echo -e "\n" | pecl install imagick \
    && pecl install redis-4.3.0 \
    && pecl install xdebug-2.5.5 \
    && pecl clear-cache \
    && docker-php-ext-enable imagick redis xdebug \
    && apt-get clean && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

#安装composer 我个人觉得这样方便使用
RUN php -r "copy('https://getcomposer.org/installer','composer-setup.php');" \
    && php composer-setup.php \
    && php -r"unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update  \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer
