FROM php:7.0-fpm

# install php modules
RUN apt-get update && apt-get install -y \
        zip unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
	&& docker-php-ext-install pdo_mysql \ 
	&& docker-php-ext-install mysqli \ 
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install gd \
	&& docker-php-ext-install zip

# install composer globally
# May need to update hash; see Command-line Installation: https://getcomposer.org/download/
WORKDIR /tmp
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');"

ENV COMPOSER_ALLOW_SUPERUSER=1
RUN mv composer.phar /usr/local/bin/composer \
    && composer global require "laravel/lumen-installer"

ENV PATH "$PATH:/root/.composer/vendor/bin"

# to get terminal access after docker-compose up -d, run: docker exec -it [Container ID] /bin/bash

