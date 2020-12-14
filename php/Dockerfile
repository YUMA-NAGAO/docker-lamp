FROM php:7.2-fpm

RUN apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends golang-go git \
    && rm -rf /var/lib/apt/lists

RUN go get github.com/mailhog/mhsendmail \
    && mv ~/go/bin/mhsendmail /usr/local/bin


RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini && \
	sed -i -e "s|;session.save_path = "/tmp"|session.save_path = "/tmp" |" /usr/local/etc/php/php.ini && \
	sed -i -e "s|;extension=php_openssl.dll|extension=php_openssl.dll |" /usr/local/etc/php/php.ini && \
	sed -i -e "/SMTP = localhost/s/SMTP = localhost/SMTP = mailhog/" /usr/local/etc/php/php.ini && \
	sed -i -e "/;sendmail_path =/s/;sendmail_path =/sendmail_path = \"\/usr\/local\/bin\/mhsendmail --smtp-addr=mailhog:1025\"/" /usr/local/etc/php/php.ini	&& \
	docker-php-ext-install pdo_mysql && \
	pecl install mailparse-3.0.2  && \
	pecl install xdebug-2.6.1 && \
	docker-php-ext-enable mailparse xdebug

COPY ./xdebug.ini $PHP_INI_DIR/conf.d/
