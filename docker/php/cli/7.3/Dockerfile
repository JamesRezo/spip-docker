ARG XDEBUG_VERSION
FROM php:7.3-cli
ARG XDEBUG_VERSION

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    git \
    unzip \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    netpbm \
    imagemagick \
    && ln -s "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini" \
    && pear config-set php_ini "$PHP_INI_DIR/php.ini" \
    && pecl install xdebug-${XDEBUG_VERSION} \
    && docker-php-ext-enable opcache xdebug \
    && docker-php-ext-configure gd --with-png-dir=/usr --with-freetype-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install -j$(nproc) gd mysqli zip \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean && apt-get autoremove \
    && pecl clear-cache \
    && ln -s "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini" \
    && mkdir /root/.composer \
    && curl -s -o /root/.composer/keys.dev.pub https://composer.github.io/snapshots.pub \
    && curl -s -o /root/.composer/keys.tags.pub https://composer.github.io/releases.pub \
    && true

COPY spip.ini /usr/local/etc/php/conf.d

COPY --from=composer:${COMPOSER_VERSION} /usr/bin/composer /usr/local/bin/composer
COPY --from=spipremix/checkout /checkout /usr/local/bin/checkout

ENV COMPOSER_ALLOW_SUPERUSER=1

WORKDIR /build
