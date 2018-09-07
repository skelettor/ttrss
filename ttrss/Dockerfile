FROM php:fpm-alpine

LABEL description="Tiny Tiny RSS Image based on Official php:fpm-alpine" \
      tags="latest" \
      maintainer="skelettor <https://github.com/skelettor>"

RUN 	apk --no-cache upgrade \
	&& apk add --no-cache git libjpeg-turbo icu-libs postgresql-libs freetype libpng \
	&& apk add --no-cache --virtual .phpext-deps libpng-dev libjpeg-turbo-dev icu-dev postgresql-dev freetype-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install gd pgsql pdo_pgsql intl pcntl \
	&& docker-php-ext-enable opcache \
	&& apk del --no-cache --purge .phpext-deps \
	&& mkdir -p /var/www/ttrss \
	&& mkdir -p /var/www/ttrss/themes/clean-greader/ \
	&& git clone https://tt-rss.org/git/tt-rss.git /var/www/tt-rss \
	&& git clone https://github.com/levito/tt-rss-feedly-theme.git /var/www/ttrss/themes \
	&& addgroup -S -g 101 nginx \
	&& adduser -D -H -S -h /var/empty -s /bin/false -u 100 -G nginx nginx \
	&& sed -i s/'user = www-data'/'user = nginx'/g /usr/local/etc/php-fpm.d/www.conf \
	&& sed -i s/'group = www-data'/'group = nginx'/g /usr/local/etc/php-fpm.d/www.conf \
	&& sed -i "s#define('PHP_EXECUTABLE', '/usr/bin/php')#define('PHP_EXECUTABLE', '/usr/local/bin/php')#g" /var/www/ttrss/config.php-dist \
	&& chown -R nginx:nginx /var/www/ttrss

VOLUME /var/www/ttrss

WORKDIR /var/www/ttrss

