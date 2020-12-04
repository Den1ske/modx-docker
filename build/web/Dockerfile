FROM php:7.4-apache

RUN a2enmod rewrite

ARG MODX_SHA1
ARG MODX_VERSION
ARG SMTP_HOST
ARG SMTP_PORT
ARG SMTP_USER
ARG SMTP_FROM
ARG SMTP_PASSWORD

RUN apt-get update && apt-get install -y \
    libpq-dev \
    libxpm-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libzip-dev \
    zip \
    msmtp \
    unzip \
    sudo && rm -rf /var/lib/apt/lists/*
RUN docker-php-ext-configure gd --with-webp --with-jpeg --with-xpm --with-freetype
RUN docker-php-ext-install gd
RUN docker-php-ext-install opcache mysqli pdo pdo_mysql

COPY ./php.ini $PHP_INI_DIR/php.ini
COPY ./.msmtprc /etc/msmtprc

RUN sed -i 's/{USER}/'${SMTP_USER}'/' /etc/msmtprc
RUN sed -i 's/{PASSWORD}/'${SMTP_PASSWORD}'/' /etc/msmtprc
RUN sed -i 's/{HOST}/'${SMTP_HOST}'/' /etc/msmtprc
RUN sed -i 's/{PORT}/'${SMTP_PORT}'/' /etc/msmtprc
RUN sed -i 's/{FROM}/'${SMTP_FROM}'/' /etc/msmtprc

RUN touch /var/log/php-mail.log
RUN touch /var/log/msmtp.log

RUN chown www-data:www-data /var/log/msmtp.log \
    && chown www-data:www-data /var/log/php-mail.log \
    && chown www-data:www-data /etc/msmtprc



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