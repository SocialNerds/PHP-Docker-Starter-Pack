FROM php:7.3-fpm

# Get user id and group id from arguments.
ARG UID
ARG GID

# Setup FPM.
RUN sed -i -e"s/^user\s=\swww-data/user = serveruser/" /usr/local/etc/php-fpm.d/www.conf && \
  sed -i -e"s/^group\s=\swww-data/group = serveruser/" /usr/local/etc/php-fpm.d/www.conf && \
  sed -i -e"s/^;php_admin_value\[memory_limit\]\s=\s32M/php_admin_value[memory_limit] = 256M\nphp_admin_value[max_input_vars] = 5000/" /usr/local/etc/php-fpm.d/www.conf && \
  echo "\ncatch_workers_output = yes\nphp_admin_value[error_log] = /var/log/fpm-php.www.log\nphp_admin_flag[log_errors] = on\n" >> /usr/local/etc/php-fpm.d/www.conf && \
  touch /var/log/fpm-php.www.log && chmod 777 /var/log/fpm-php.www.log && \
  # User.
  groupadd -r serveruser -g $GID && useradd -ms /bin/bash serveruser -u $UID -g $GID && \
  apt-get update && apt-get install -y \
  libpng-dev libjpeg-dev git wget nano libzip-dev zip unzip mysql-client && \
  # Other packages.
  docker-php-ext-install pdo_mysql && \
  docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ && \
  docker-php-ext-install gd && \
  docker-php-ext-configure zip --with-libzip && \
  docker-php-ext-install zip && \
  # Composer.
  cd ~ && \
  curl -sS https://getcomposer.org/installer -o composer-setup.php && \
  php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
  rm -rf composer-setup.php && \
  # PHPUnit.
  wget https://phar.phpunit.de/phpunit.phar && \
  chmod +x phpunit.phar && \
  mv phpunit.phar /usr/local/bin/phpunit && \
  # Opcache.
  docker-php-ext-configure opcache --enable-opcache && \
  docker-php-ext-install opcache && \
  # Xdebug.
  pecl install xdebug && docker-php-ext-enable xdebug

# Copy Opcache config.
COPY config/opcache.ini /usr/local/etc/php/conf.d/

# Copy Xdebug config.
COPY config/xdebug.ini /usr/local/etc/php/conf.d/

USER serveruser
RUN echo "PATH=$PATH:~/.composer/vendor/bin" >> ~/.bashrc && \
  composer global require laravel/installer

WORKDIR /home/serveruser/www/project
USER root

ENTRYPOINT ["php-fpm"]
