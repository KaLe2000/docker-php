FROM phpdockerio/php:8.1-fpm
ARG XDEBUG_PORT
ARG USER_UID
ARG USER_NAME
VOLUME /var/www/

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install bash-completion ssh iproute2 iputils-ping mc wget nano php8.1-curl php8.1-pdo-mysql php8.1-redis php8.1-xdebug php8.1-bcmath php8.1-gd php8.1-imagick php8.1-intl

# Install git
RUN apt-get update \
    && apt-get -y --no-install-recommends install git \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN touch /run/php-fpm.pid \
    && useradd -d /home/dockeruser -u $USER_UID -ms /bin/bash $USER_NAME \
    && chown $USER_UID:$USER_UID /run/php-fpm.pid

COPY . /var/www
COPY ./docker/php/xdebug.ini  /usr/local/etc/php/conf.d/xdebug.ini
COPY ./docker/php/xdebug.ini  /etc/php/8.1/mods-available/xdebug-docker.ini

RUN phpenmod php-ini-config \
    && phpenmod xdebug-docker

EXPOSE $XDEBUG_PORT

USER ${USER_UID}
