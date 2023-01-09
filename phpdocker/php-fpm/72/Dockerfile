FROM php:7.2-fpm

LABEL maintainer="chao <zhilan.cc@gamil.com>"
LABEL version="1.0"

WORKDIR "/application"

ENV TZ=Asia/Shanghai
ENV SWOOLE_VERSION=4.5.2

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

RUN install-php-extensions bcmath gd pdo_mysql sockets sysvmsg sysvsem sysvshm xdebug xmlrpc xsl yaml zip mysqli memcached mongodb redis xhprof soap shmop intl gettext pcntl exif apcu oci8

RUN wget https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz -O swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-mysqlnd --enable-sockets --enable-openssl --enable-http2 \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

COPY ./deploy/swoole_loader72.so /usr/local/lib/php/extensions/swoole_loader72.so
COPY ./conf/swoole_loader72.ini /usr/local/etc/php/conf.d/swoole_loader72.ini

EXPOSE 9000

CMD ["php-fpm"]
