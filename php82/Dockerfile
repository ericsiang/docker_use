FROM php:8.2-fpm-alpine

WORKDIR /var/www/project

# COPY ./config/supervisord/supervisord.conf /etc/supervisord.conf
# COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
# COPY ./config/nginx/nginx-default.conf /etc/nginx/http.d/default.conf
# COPY ./config/nginx/index.html /var/www/html/index.html

RUN apk update \
    # 常用套件安裝
    && apk add --no-cache zip unzip dos2unix supervisor libpng-dev libzip-dev freetype-dev $PHPIZE_DEPS libjpeg-turbo-dev tzdata git\
    # composer安裝
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer --ansi --version --no-interaction \
    # php extension安裝
    && docker-php-ext-install gd pcntl bcmath mysqli pdo_mysql \
    # php extension設定
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    # 透過pecl安裝php extension
    && pecl install zip \
    && docker-php-ext-enable zip \
    && pecl install igbinary \
    && docker-php-ext-enable igbinary \
    && pecl install redis   \
    && docker-php-ext-enable redis  \
    # imagemagick
    && apk add imagemagick imagemagick-dev \
    && printf "\n" | pecl install -o -f imagick \
    && docker-php-ext-enable imagick \
    && php -m | grep imagick \
    # php ini setting
    && mv /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini. 
    # nginx
    # && apk add nginx \
    # && ln -sf /dev/stdout /var/log/nginx/access.log \
    # && ln -sf /dev/stderr /var/log/nginx/error.log 
    
# EXPOSE 80

# container啟動時執行supervisor
# CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]