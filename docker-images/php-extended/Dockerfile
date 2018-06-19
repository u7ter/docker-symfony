FROM php:5.6-fpm
MAINTAINER Vladislav Lotysh <lotysh.vm@gmail.com>

RUN apt-get update
# update the repository sources list
# and install dependencies
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get install -y git \
    && apt-get -y autoclean

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 8.4
ENV VAR_DUMPER_VERSION 3.0.3

# install nvm
# https://github.com/creationix/nvm#install-script
#RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
#RUN chmod u+x $NVM_DIR/nvm.sh
#RUN ls -la $NVM_DIR
#RUN $NVM_DIR/nvm.sh
#   RUN nvm install $NODE_VERSION
#   RUN nvm alias default $NODE_VERSION
#   RUN nvm use default

# Install nvm with node and npm
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

RUN apt-get install -y software-properties-common

# confirm installation
#RUN node -v
#RUN npm -v

# docker installa
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install composer, drush and other tools.
ENV COMPOSER_HOME /composer-libs
RUN mkdir /composer-libs \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer global require symfony/var-dumper:${VAR_DUMPER_VERSION} \
    && chown -R www-data:www-data /composer-libs


#apt-get -o Debug::pkgProblemResolver=yes dist-upgrade

RUN apt-get upgrade -y
RUN apt-get -f -y install
RUN apt-get install -y libxml2-dev libxslt1-dev python-dev
RUN apt-get install -y systemd

#compass
RUN apt-get install -y ruby ruby-dev \
    && gem update --system \
    && gem install compass \
    && gem install compass

RUN apt-get install -y redis-server
#RUN apt-get install -y php-redis
RUN systemctl enable redis-server.service



#gearman
RUN apt-get update \
    && apt-get install -y gearman

#install redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

# Some basic extensions
RUN docker-php-ext-install -j$(nproc) json mbstring opcache pdo pdo_mysql mysqli

# Curl
RUN apt-get install -y libcurl4-openssl-dev
RUN docker-php-ext-install -j$(nproc) curl

# GD
RUN apt-get install -y libpng-dev libjpeg-dev
RUN docker-php-ext-install -j$(nproc) gd

# Intl
RUN apt-get install -y libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

#install Imagemagick & PHP Imagick ext
RUN apt-get update && apt-get install -y \
  libmagickwand-dev --no-install-recommends
RUN pecl install imagick && docker-php-ext-enable imagick

# Compile and install xDebug
#RUN pecl install xdebug \

RUN pecl install xdebug-2.5.5
RUN docker-php-ext-enable xdebug

#install exif
RUN docker-php-ext-install exif

RUN apt-get install -y libxslt1-dev

#install xsl
RUN docker-php-ext-install xsl \
    && docker-php-ext-enable xsl

RUN apt-get install -y libgearman-dev
#install gearman
RUN pecl install gearman
RUN docker-php-ext-enable gearman


#CMD service redis-server start
COPY ./start.sh /code/docker-images/php-extended/start.sh
#ENTRYPOINT  ["/usr/bin/redis-server"]
#USER root
#RUN ["chmod", "+x", "/code/docker-images/php-extended/start.sh"]
#ENTRYPOINT ["sh", "/code/docker-images/php-extended/start.sh"]
#RUN ["chmod", "+x", "/code/docker-images/php-extended/start.sh"]
#CMD ["chmod", "+x", "/code/docker-images/php-extended/start.sh"]
#CMD "/code/docker-images/php-extended/start.sh"
#CMD ["service", "redis-server start"]