FROM php:5.6-fpm

LABEL maintainer="chao <zhilan.cc@gamil.com>"
LABEL version="1.0"

WORKDIR "/application"

ENV TZ=Asia/Shanghai

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y \
        curl wget git zip unzip less vim procps lsof tcpdump htop openssl net-tools iputils-ping \
        libz-dev \
        libssl-dev \
        libnghttp2-dev \
        libpcre3-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN install-php-extensions bcmath gd pdo_mysql sockets sysvmsg sysvsem sysvshm xdebug xmlrpc xsl yaml zip mysqli memcached mongodb redis xhprof soap shmop intl gettext pcntl exif

EXPOSE 9000

CMD ["php-fpm"]