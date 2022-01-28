FROM php:8.0-fpm


RUN apt update && apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip make  \
    && docker-php-ext-install intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN apt-get clean && rm -rf /var/lib/apt/lists/*


WORKDIR /var/www/backend

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ARG user=admin
ARG group=admin
ARG uid=1000
ARG gid=1000
RUN groupadd -g ${gid} ${group}
RUN useradd -u ${uid} -ms /bin/bash -g ${group} ${user}
COPY --chown=${group}:${user} . /var/www
##RUN curl -sS https://get.symfony.com/cli/installer | bash
##RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

ADD docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/
RUN echo "include_path=/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini" >> /usr/local/etc/php/php.ini

##RUN git config --global user.email "yaroslav.bailo@gmail.com" \
   ## && git config --global user.name "Yaroslav"
