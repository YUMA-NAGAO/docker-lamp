FROM httpd:2.4.41

RUN apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends openssl \
    && rm -rf /var/lib/apt/lists

RUN sed -i -e "/mod_proxy.so/s/^#//" /usr/local/apache2/conf/httpd.conf \
    && sed -i -e "/mod_proxy_fcgi.so/s/^#//" /usr/local/apache2/conf/httpd.conf \
    && sed -i -e "/mod_ssl.so/s/^#//" /usr/local/apache2/conf/httpd.conf \
    && echo "Include conf/extra/docker.conf" >> /usr/local/apache2/conf/httpd.conf

RUN mkdir -p /etc/ssl/private && \
    openssl req -new -newkey rsa:2048 -nodes -out /etc/ssl/private/server.csr -keyout /etc/ssl/private/server.key -subj "/C=/ST=/L=/O=/OU=/CN=*.lvh.me" && \
    openssl x509 -days 3650 -req -signkey /etc/ssl/private/server.key -in /etc/ssl/private/server.csr -out /etc/ssl/private/server.crt


COPY docker.conf /usr/local/apache2/conf/extra

# dockre can not use systemd without permission, apahce is started with httpd
#CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
