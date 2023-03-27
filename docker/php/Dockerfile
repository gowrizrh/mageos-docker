FROM php:8.1.4-fpm-bullseye

ENV UID=1062032646
ENV GID=1062000513

RUN groupadd -g ${GID} --system mygroup
RUN useradd --no-log-init --create-home --shell /bin/bash --system -s /bin/sh -g mygroup -u ${UID} myuser

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions @composer opcache gd bcmath zip intl xsl pdo_mysql soap sockets

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
