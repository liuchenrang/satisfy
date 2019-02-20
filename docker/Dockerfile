FROM php:7.1-apache

RUN apt-get update -q \
    && apt-get install -qy git cron supervisor \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD conf/supervisor/* /etc/supervisor/conf.d/
ADD conf/cron.conf /etc/cron.d/satisfy
ADD conf/apache.conf /etc/apache2/sites-available/000-default.conf
ADD conf/php.ini /usr/local/etc/php/conf.d/php.ini
RUN a2enmod rewrite

RUN mkdir /var/www/.ssh && chown -R www-data:www-data /var/www

# below line because default image no home dir and crond update satis cound execute shell
RUN usermod -s /bin/bash www-data
RUN usermod -d /home/www-data  www-data
RUN rm -rf /home/www-data 
RUN mkdir /home/www-data && chown -Rf www-data:www-data /home/www-data  && su - www-data -c "ssh-keygen"
#
VOLUME /var/www/.ssh

# symbolic links to index.php and static files
ARG APP_PATH=/var/www/satisfy

WORKDIR /var/www/satisfy

ENTRYPOINT ["supervisord", "-n"]
