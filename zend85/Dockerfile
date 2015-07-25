# Zend Server
#
# Version 0.2

# TODO:

FROM ubuntu:14.04

RUN apt-key adv --keyserver pgp.mit.edu --recv-key 799058698E65316A2E7A4FF42EAE1437F7D2C623
RUN echo "deb http://repos.zend.com/zend-server/8.5/deb_apache2.4 server non-free" >> /etc/apt/sources.list.d/zend-server.list
RUN apt-get update && apt-get install -y curl git zend-server-php-5.6 && /usr/local/zend/bin/zendctl.sh stop
#RUN apt-get update && apt-get install -y git zend-server-php-5.6

#COPY ./zend.lic /etc/

RUN git clone https://github.com/beejhuff/zs-init.git /usr/local/zs-init
WORKDIR /usr/local/zs-init
RUN /usr/local/zend/bin/php -r "readfile('https://getcomposer.org/installer');" | /usr/local/zend/bin/php
RUN /usr/local/zend/bin/php composer.phar update

#RUN sed -i '1s/;//' /usr/local/zend/etc/conf.d/pcntl.ini

COPY ./scripts /usr/local/bin
COPY ./libmysqlclient.so.18 /usr/lib/x86_64-linux-gnu/
COPY ./Zray /usr/local/zend/var/plugins/

RUN apt-get install -y unzip

RUN rm -rf /var/www/html/* \
    && curl -o /tmp/shopware.zip -L http://releases.s3.shopware.com.s3.amazonaws.com/install_5.0.2_f003e4550105c830fc6ca87b340ffa0aba1ae969.zip \
    && unzip -o /tmp/shopware.zip -d /var/www/html \
    && curl -o /tmp/test_images.zip -L http://releases.s3.shopware.com/test_images.zip \
    && unzip -o /tmp/test_images.zip -d /var/www/html \
    && chmod -R 755 /var/www/html/logs \
    && chmod -R 755 /var/www/html/cache \
    && chmod -R 755 /var/www/html/web \
    && chmod -R 755 /var/www/html/files \
    && chmod -R 755 /var/www/html/media \
    && chmod -R 755 /var/www/html/engine/Shopware/Plugins/Community \
    && rm /tmp/test_images.zip \
    && rm /tmp/shopware.zip

COPY ./config.php /var/www/html/
COPY ./vhost.conf /usr/local/zend/etc/sites.d/vhost_http_localhost_80.conf

RUN ln -s /usr/local/zend/bin/php /bin/php

RUN chmod 755 /var/www/html/config.php
RUN chown www-data:www-data /var/www/html -R

# fix shopware install issue
RUN chmod +x /var/www/html/bin/console \
    && echo "<?php exec('php /var/www/html/bin/console sw:generate:attributes'); exec('chown www-data:www-data /var/www/html -R'); ?>" >> /var/www/html/recovery/install/templates/configuration.php

RUN mkdir -p /usr/local/zend/etc/sites.d/http/localhost/80

EXPOSE 80
EXPOSE 443
EXPOSE 10081
EXPOSE 10082

CMD ["/usr/local/bin/run"]
#CMD ["/usr/local/zs-init/init.php"]
#CMD ["/usr/local/zs-init/nothing.php", "/usr/local/bin/nothing"]
