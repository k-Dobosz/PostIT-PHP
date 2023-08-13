FROM php:8.2-apache

RUN a2enmod rewrite

RUN apt-get update \
  && apt-get install -y libzip-dev git wget --no-install-recommends \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN docker-php-ext-install pdo mysqli pdo_mysql zip;

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY apache.conf /etc/apache2/sites-enabled/000-default.conf
COPY setup.sh /usr/local/bin/setup.sh

RUN chmod +x /usr/local/bin/setup.sh

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

WORKDIR /var/www/symfony

ENTRYPOINT ["/usr/local/bin/setup.sh"]

CMD ["apache2-foreground"]