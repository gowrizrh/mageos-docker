FROM php:8.1.4-fpm-bullseye as php

ARG UID ${UID}
ARG GID ${GID}

RUN groupadd -g ${GID} --system mygroup
RUN useradd --no-log-init --create-home --shell /bin/bash --system -s /bin/bash -g mygroup -u ${UID} myuser

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions @composer opcache gd bcmath zip intl xsl pdo_mysql soap sockets

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get --no-install-recommends --assume-yes --quiet install nodejs \
    && rm -rf /var/lib/apt/lists/*

ADD https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 /usr/local/bin/mhsendmail
RUN chmod +x /usr/local/bin/mhsendmail

RUN sed -i "s/user = www-data/user = myuser/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/group = www-data/group = mygroup/g" /usr/local/etc/php-fpm.d/www.conf
RUN echo "memory_limit = -1" > $PHP_INI_DIR/conf.d/php-memory-limits.ini
RUN echo "max_execution_time = 120" >> $PHP_INI_DIR/conf.d/php-memory-limits.ini
RUN echo "sendmail_path = '/usr/local/bin/mhsendmail --smtp-addr=\"mail:1025\"'" >> $PHP_INI_DIR/conf.d/php-memory-limits.ini

RUN mkdir -p /var/www/html

USER myuser

WORKDIR /var/www/html

CMD ["php-fpm", "-y", "/usr/local/etc/php-fpm.conf", "-R"]

FROM php as php-debug

USER root

RUN install-php-extensions xdebug

RUN cat <<EOF > $PHP_INI_DIR/conf.d/php-xdebug.ini
xdebug.mode = debug,profile
xdebug.client_host = host.docker.internal
xdebug.client_port = 9003
xdebug.idekey = PHPSTORM
xdebug.output_dir=/var/www/html
xdebug.start_with_request=trigger 
EOF

USER myuser
