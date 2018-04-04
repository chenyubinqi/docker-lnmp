FROM chenyubinqi/docker-php

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install sysvmsg sysvsem sysvshm

RUN curl -fsSL 'http://pecl.php.net/get/swoole-2.1.1.tgz' -o swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
        cd swoole \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

RUN apt-get update && apt-get install -y zip unzip --no-install-recommends && rm -rf /var/lib/apt/lists/* \
    && curl -fsSL 'https://github.com/krakjoe/pthreads/archive/master.zip' -o pthreads.zip \
    && mkdir -p pthreads \
    && unzip pthreads.zip -d pthreads \
    && rm pthreads.zip \
    && ( \
        cd pthreads/pthreads-master \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r pthreads

COPY php.ini $PHP_INI_DIR
RUN cd $PHP_INI_DIR && cp php.ini php-cli.ini && echo -e "\nextension=pthreads.so" >> php-cli.ini

ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer

WORKDIR /opt
