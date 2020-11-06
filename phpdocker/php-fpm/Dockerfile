FROM phpdockerio/php7-fpm:latest
WORKDIR "/application"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install automake bison flex g++ libboost-all-dev libevent-dev libssl-dev libtool make git nodejs npm wget pkg-config apt-utils build-essential php-pear php-dev php-bcmath php-mysql php-redis php-bz2 php-gd php-imagick php-intl php-soap php-xdebug php-apcu  \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install ext-mongodb
RUN pecl install mongodb 
RUN echo 'extension=mongodb.so' > /etc/php/7.0/fpm/conf.d/20-mongodb.ini \
   && ln -s /etc/php/7.0/fpm/conf.d/20-mongodb.ini /etc/php/7.0/cli/conf.d/20-mongodb.ini

# update node
RUN apt-get -y --no-install-recommends install openssl \
    &&  export NODE_MIRROR=https://npm.taobao.org/dist/
RUN ln -s /usr/bin/nodejs /usr/bin/node \
    && npm config set registry https://registry.npm.taobao.org \
    && npm install npm@latest -g \
    && npm install -g n \
    && n stable \
    && rm -rf /usr/bin/node \
    && ln -s /usr/local/bin/node /usr/bin/node

# Install Global npm package
RUN npm install -g bower \
    && npm install -g gulp

# Install ext-zookeeper
RUN cd /tmp \
    && wget http://archive.apache.org/dist/zookeeper/zookeeper-3.4.12/zookeeper-3.4.12.tar.gz \
    && tar zxvf zookeeper-3.4.12.tar.gz\
    && cd zookeeper-3.4.12/src/c \
    && ./configure --prefix=/usr/local/zookeeper \
    && make && make install

RUN cd /tmp \
    && wget http://pecl.php.net/get/zookeeper-0.6.4.tgz \
    && tar zxvf zookeeper-0.6.4.tgz \
    && cd zookeeper-0.6.4 \
    && phpize \
    && ./configure --with-php-config=/usr/bin/php-config7.0 --with-libzookeeper-dir=/usr/local/zookeeper \
    && make && make install
RUN echo 'extension=zookeeper.so' > /etc/php/7.0/fpm/conf.d/20-zookeeper.ini \
   && ln -s /etc/php/7.0/fpm/conf.d/20-zookeeper.ini /etc/php/7.0/cli/conf.d/20-zookeeper.ini


# Install thrift
RUN cd /tmp \
    &&  wget  https://mirror.bit.edu.cn/apache/thrift/0.11.0/thrift-0.11.0.tar.gz \
    &&  apt-get update \
    &&  apt-get -y install  \
    && tar -zxvf thrift-0.11.0.tar.gz \
    && cd thrift-0.11.0 \
    && ./configure --without-nodejs \
    && make && make install

# Install composer
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && mv composer.phar /usr/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com
