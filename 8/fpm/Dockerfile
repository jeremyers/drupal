# from https://www.drupal.org/requirements/php#drupalversions
FROM php:5.6-fpm

# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev libpq-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
	&& docker-php-ext-install gd mbstring opcache pdo pdo_mysql pdo_pgsql zip

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=60'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini


VOLUME /var/www/html

# https://www.drupal.org/node/3060/release
ENV DRUPAL_VERSION 8.0.3
ENV DRUPAL_MD5 7d5f5278a870b8f4a29cda4fe915d619



RUN curl -fSL "http://ftp.drupal.org/files/projects/drupal-${DRUPAL_VERSION}.tar.gz" -o /usr/src/drupal-${DRUPAL_VERSION}.tar.gz

#Move in the entrypoint script
COPY drupal_entrypoint.sh /drupal_entrypoint.sh

#Define data volumes
#VOLUME ["/var/www/html/modules", "/var/www/html/profiles", "/var/www/html/themes", "/var/www/html/sites"]

#Set the entrypoint
ENTRYPOINT ["/drupal_entrypoint.sh"]
CMD ["php-fpm"]
