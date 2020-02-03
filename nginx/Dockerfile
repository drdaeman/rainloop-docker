FROM nginx:alpine

# Copy configuration
COPY default.conf /etc/nginx/conf.d/default.conf

# Set up working directory
WORKDIR /var/www/html

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
 && rm /tmp/rainloop.zip /tmp/rainloop.zip.asc
