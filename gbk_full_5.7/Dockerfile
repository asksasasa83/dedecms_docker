FROM php:5.6-apache

# install the PHP extensions we need
RUN set -ex; \
	\
	apt-get update; \
	apt-get install -y \
		libjpeg-dev \
		libpng-dev \
	; \
	rm -rf /var/lib/apt/lists/*; \
	\
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install gd mysqli opcache mysql
# TODO consider removing the *-dev deps and only keeping the necessary lib* packages

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN a2enmod rewrite expires

VOLUME /var/www/html

ENV DEDECMS_ENCODING GBK

RUN set -ex; \
	curl -o dedecms.tar.gz -fSL "http://updatenew.dedecms.com/base-v57/package/DedeCMS-V5.7-${DEDECMS_ENCODING}-SP2-Full.tar.gz"; \
# upstream tarballs include ./dedecms/ so this gives us /usr/src/dedecms
	tar -xzf dedecms.tar.gz -C /usr/src/; \
	mv /usr/src/DedeCMS-V5.7-${DEDECMS_ENCODING}-SP2-Full/uploads /usr/src/dedecms; \
	rm dedecms.tar.gz; \
	rm -r /usr/src/DedeCMS-V5.7-${DEDECMS_ENCODING}-SP2-Full; \
	chown -R www-data:www-data /usr/src/dedecms

COPY docker-entrypoint.sh /usr/local/bin/

RUN set -ex; \
	chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["apache2-foreground"]