#
FROM php:7.1-cli

LABEL MAINTAINER="Allan Freitas <allankode@gmail.com>"

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libaio1 \
    unzip && rm -rf /var/lib/apt/lists/*

#
RUN docker-php-ext-configure gd --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install mcrypt pdo_mysql pdo_pgsql gd \
    && rm -rf /var/lib/apt/lists/*

#####################################
# Install OCI8:
#####################################
RUN cd /tmp/ \
    && curl -LJO https://github.com/allanfreitas/dphpoci8/raw/master/sharedlibs/instantclient-basic-linux.x64-12.2.0.1.0.zip \
    && curl -LJO https://github.com/allanfreitas/dphpoci8/raw/master/sharedlibs/instantclient-sdk-linux.x64-12.2.0.1.0.zip \
    && unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/ \
    && rm -Rf /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip \
    && rm -Rf /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip \
    && ln -s /usr/local/instantclient_12_2 /usr/local/instantclient \
    && ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so \
    && ln -s /usr/local/instantclient/libocci.so.12.1 /usr/local/instantclient/libocci.so \
    && docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient \
    && docker-php-ext-install oci8 \
    && rm -rf /var/lib/apt/lists/*