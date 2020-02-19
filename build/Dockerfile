# nginx + php-fpm + mediainfo-php
FROM        ubuntu:16.04

# set up environment vars and correct locale
ENV         OS_LOCALE="en_US.UTF-8" \
            DEBIAN_FRONTEND=noninteractive \
            TZ=America/New_York

RUN         apt-get update && apt-get install -y locales && locale-gen ${OS_LOCALE}

ENV         LANG=${OS_LOCALE} \
            LANGUAGE=${OS_LOCALE} \
            LC_ALL=${OS_LOCALE}

# install nginx, mediainfo, php-fpm
# newer versions of mediainfo do not work with php-mediainfo
RUN         apt-get update && apt-get install -y \
                php \
		nginx \
		mediainfo \ 
		php-fpm \
                php-mysqli \
		php-json \
                apt-transport-https \
		ca-certificates \
		openssl \
		wget \
		unzip \
		git \
		php-curl php-xml php-phar php-intl php-dom php-xmlreader php-ctype php-mbstring php-gd curl

# setup supervisor.d
RUN         apt-get update && apt-get install -y supervisor && mkdir -p /var/log/supervisor \
		&& mkdir /run/php && chown www-data:www-data /run/php \
                && ln -sf /dev/stdout /var/log/nginx/access.log \
		&& ln -sf /dev/stderr /var/log/nginx/error.log


COPY        conf/supervisord.conf /etc/supervisor/conf.d/
COPY        conf/default /etc/nginx/sites-available/

WORKDIR     /var/www/webmshare/

EXPOSE      80

ENTRYPOINT  [ "/usr/bin/supervisord" ]
