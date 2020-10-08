FROM php:7.4-apache

RUN a2enmod rewrite


RUN apt-get update && apt-get install -y libpng-dev libjpeg-dev unzip sudo && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-configure gd \
	&& docker-php-ext-install gd opcache mysqli pdo pdo_mysql
    
ADD ./php.ini /etc/php/7.4/conf.d/90-php.ini

ARG MODX_SHA1
ARG MODX_VERSION


VOLUME /var/www/html

# upstream tarballs include ./modx-${MODX_VERSION}/
RUN curl -o modx.zip -SL http://modx.com/download/direct/modx-${MODX_VERSION}-pl.zip \
	&& echo "$MODX_SHA1 *modx.zip" | sha1sum -c - \
	&& unzip modx.zip -d /usr/src \
  && mv /usr/src/modx-${MODX_VERSION}-pl /usr/src/modx \
  && find /usr/src/modx -name 'ht.access' -exec bash -c 'mv $0 ${0/ht.access/.htaccess}' {} \; \
  && rm modx.zip \
	&& chown -R www-data:www-data /usr/src/modx

COPY ./docker-entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]