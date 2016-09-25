FROM php-zendserver:9.0-php7

RUN apt-get install -y unzip \
    && rm -rf /var/www/html/* \
    && curl -o /tmp/shopware.zip -L http://releases.s3.shopware.com.s3.amazonaws.com/install_5.2.7_b2081fd410306d74001ae1a2ce725b003c3e0259.zip \
    && unzip -o /tmp/shopware.zip -d /var/www/html \
    && curl -o /tmp/test_images.zip -L http://releases.s3.shopware.com/test_images.zip \
    && unzip -o /tmp/test_images.zip -d /var/www/html \
    && chmod -R 755 /var/www/html/var \
    && rm /tmp/test_images.zip \
    && rm /tmp/shopware.zip

RUN ln -s /usr/local/zend/bin/php /bin/php \
    && chmod 755 /var/www/html/config.php \
    && chown www-data:www-data /var/www/html -R \
    && chmod +x /var/www/html/bin/console \
    && echo "<?php exec('php /var/www/html/bin/console sw:generate:attributes'); exec('chown www-data:www-data /var/www/html -R'); ?>" >> /var/www/html/recovery/install/templates/configuration.php \
    && mkdir -p /usr/local/zend/etc/sites.d/http/localhost/80 \
    && echo "" >> /usr/local/zend/etc/php.ini \
    && echo "date.timezone = Europe/Berlin" >> /usr/local/zend/etc/php.ini

COPY ./config.php /var/www/html/
COPY ./vhost.conf /usr/local/zend/etc/sites.d/vhost_http_localhost_80.conf
