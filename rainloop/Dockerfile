FROM php:7-fpm-alpine

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# Install Rainloop
ARG RAINLOOP_VERSION=1.14.0
ARG RAINLOOP_PUBLIC_KEY=3B797ECE694F3B7B70F311A4ED7C49D987DA4591
ARG KEYSERVER=ha.pool.sks-keyservers.net
RUN wget -O /tmp/rainloop.zip "https://github.com/RainLoop/rainloop-webmail/releases/download/v$RAINLOOP_VERSION/rainloop-community-$RAINLOOP_VERSION.zip" \
 && wget -O /tmp/rainloop.zip.asc "https://github.com/RainLoop/rainloop-webmail/releases/download/v$RAINLOOP_VERSION/rainloop-community-$RAINLOOP_VERSION.zip.asc" \
 && apk add --no-cache --virtual .build-deps gnupg \
 && export GNUPGHOME="$(mktemp -d)" \
 && gpg --batch --keyserver "$KEYSERVER" --recv-keys "$RAINLOOP_PUBLIC_KEY" \
 && gpg --verify /tmp/rainloop.zip.asc \
 && gpgconf --kill all \
 && rm -rf "$GNUPGHOME" \
 && unzip /tmp/rainloop.zip -d /var/www/html \
 && apk del --no-network .build-deps \
 && rm /tmp/rainloop.zip /tmp/rainloop.zip.asc \
 && chown -R www-data:www-data /var/www/html/data

# Run as non-root
USER www-data

# Persist configuration in a volume
VOLUME ["/var/www/html/data"]
