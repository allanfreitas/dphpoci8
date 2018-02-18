#
FROM php:7.1-cli-alpine

LABEL MAINTAINER="Allan Freitas <allankode@gmail.com>"

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN set -ex && apk upgrade -U -a && apk add \
        #--no-cache --virtual .build-deps \
    curl \
    libmemcached-dev \
    libzdb \
    libpq \
    libjpeg \
    libpng-dev \
    freetype-dev \
    postgresql-dev \ 
    libjpeg-turbo-dev \
    #libssl-dev \
    libmcrypt-dev \
    unzip libaio libaio-dev

#
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-freetype-dir=/usr \
    --with-png-dir=/usr \
    --with-jpeg-dir=/usr \
    && docker-php-ext-install mcrypt pdo_mysql pdo_pgsql gd 
    #\
  #&& docker-php-ext-install pdo_mysql \
  #&& docker-php-ext-install pdo_pgsql \
    #--with-freetype-dir=/usr/include/freetype2 \
   # && docker-php-ext-install gd 
    #\
    #&& apk del .build-deps

#####################################
# Install OCI8:
#####################################
ADD ./sharedlibs/instantclient-basic-linux.x64-12.2.0.1.0.zip /tmp/
ADD ./sharedlibs/instantclient-sdk-linux.x64-12.2.0.1.0.zip /tmp/

RUN unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/ \
    && ln -s /usr/local/instantclient_12_2 /usr/local/instantclient

#RUN cd /usr/local/instantclient && ls -lah

#RUN echo '----------------------------------------'

RUN ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so \
    && ln -s /usr/local/instantclient/libocci.so.12.1 /usr/local/instantclient/libocci.so

RUN docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient \
    && docker-php-ext-install oci8

RUN rm -rf /var/lib/apt/lists/*