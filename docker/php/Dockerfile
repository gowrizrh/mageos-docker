FROM php:8.2-fpm-bullseye as php

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions @composer opcache gd bcmath zip intl xsl pdo_mysql soap sockets

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get --no-install-recommends --assume-yes --quiet install nodejs \
    && rm -rf /var/lib/apt/lists/*

COPY --chown=root:root config /

ADD https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 /usr/local/bin/mhsendmail

RUN chmod +x /usr/local/bin/mhsendmail

RUN mkdir -p /var/www/html

ARG UID ${UID}
ARG GID ${GID}

RUN groupadd -g ${GID} --system admin
RUN useradd --no-log-init --create-home --shell /bin/bash --system -s /bin/bash -g admin -u ${UID} admin

USER admin

WORKDIR /var/www/html

CMD ["php-fpm", "-y", "/usr/local/etc/php-fpm.conf", "-R"]

FROM php as php-debug

USER root

RUN install-php-extensions xdebug

COPY --chown=root:root config-debug /

USER admin
