FROM php:8.1.16-fpm

LABEL maintainer="chao <zhilan.cc@gamil.com>"
LABEL version="1.0"

WORKDIR "/application"

ENV TZ=Asia/Shanghai

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY --from=composer/composer /usr/bin/composer /usr/bin/composer

RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN install-php-extensions bcmath gd pdo_mysql sockets sysvmsg sysvsem sysvshm xdebug xmlrpc zip mysqli memcached mongodb redis soap shmop gettext pcntl swoole

EXPOSE 9000

CMD ["php-fpm"]
