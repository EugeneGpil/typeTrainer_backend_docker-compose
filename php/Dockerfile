FROM ubuntu:20.04

USER root
ARG UID=1000
ARG GID=1000
ARG PHP_VERSION=8.0
ARG COMPOSER_VERSION=2.1.11
RUN groupadd -g ${GID} app && \
    useradd -u ${UID} -m app -g app && \
    apt-get update && \
    apt-get install software-properties-common -y && \
    add-apt-repository ppa:ondrej/php -y && \
    apt-get install php${PHP_VERSION} -y && \
    apt-get install php${PHP_VERSION}-cli -y && \
    apt-get install php${PHP_VERSION}-common -y && \
    apt-get install php${PHP_VERSION}-opcache -y && \
    apt-get install php${PHP_VERSION}-mysql -y && \
    apt-get install php${PHP_VERSION}-mbstring -y && \
    apt-get install php${PHP_VERSION}-mcrypt -y && \
    apt-get install php${PHP_VERSION}-zip -y && \
    apt-get install php${PHP_VERSION}-gd -y && \
    apt-get install php${PHP_VERSION}-fpm -y && \
    apt-get install php${PHP_VERSION}-xml -y && \
    apt-get install php${PHP_VERSION}-dev -y && \
    apt-get install php${PHP_VERSION}-mongodb -y && \
    apt-get install php${PHP_VERSION}-curl -y && \
    apt-get install git -y && \
    apt-get install curl -y && \
    apt-get install nano -y&& \
    echo "listen = 9000" >> /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer self-update ${COMPOSER_VERSION} && \
    curl -fsSL https://code-server.dev/install.sh | sh && \
    apt-get install php${PHP_VERSION}-intl -y

ARG PHP_VERSION=8.0
RUN pecl install mongodb && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/mongodb.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-mongodb.ini

ARG INSTALL_XDEBUG=false
ARG PHP_VERSION=8.0
RUN if [ "$INSTALL_XDEBUG" = "true" ] ; then \
        apt-get install php${PHP_VERSION}-xdebug -y && \
        echo "\n\
            xdebug.remote_autostart = 1 \n\
            xdebug.remote_enable = 1 \n\
            xdebug.remote_port = 9050 \n\
        " >> /etc/php/${PHP_VERSION}/fpm/conf.d/20-xdebug.ini && \
        echo "\n\
            zend_extension=xdebug \n\
            xdebug.mode=debug \n\
            xdebug.client_port=9050 \n\
            xdebug.discover_client_host=1 \n\
            xdebug.client_host=localhost \n\
            xdebug.start_with_request=yes \n\
        " >> /etc/php/${PHP_VERSION}/cli/php.ini \
    ; fi