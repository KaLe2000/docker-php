FROM phpdockerio/php:8.1-fpm
VOLUME /var/www/

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install bash-completion cron ssh iproute2 iputils-ping mc wget nano php8.1-curl php8.1-pdo-mysql php8.1-redis php8.1-xdebug php8.1-bcmath php8.1-gd php8.1-imagick php8.1-intl

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./docker/supervisor/crontab.cfg /var/spool/cron/crontabs/root

CMD ["cron", "-f"]
