FROM php:7.4-apache

RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update \
  && apt-get install -y \
    git \
    libmagickwand-dev \
    libreoffice-core \
    libreoffice-common \
    libreoffice-writer \
    libreoffice-script-provider-python \
    python \
  && apt-get autoremove -yqq --purge \
  && apt-get clean \
  && rm -Rf /var/lib/apt/lists/*
  
RUN  pecl install imagick \
	&& docker-php-ext-enable imagick \
  && docker-php-ext-install mysqli

RUN a2enmod headers rewrite deflate

# MySQL commandline is necessary for install, but not for running?
# MySQL v8 container uses cached_sha2 passwords, which aren't supported by
# any of the mysql clients in debian.  Use the upstream community package
# instead.
COPY mysql.gpg /tmp/mysql.gpg
RUN apt-key add /tmp/mysql.gpg \
  && apt-key adv --keyserver pgp.mit.edu --recv-keys 5072E1F5 \
  && rm /tmp/mysql.gpg
COPY mysql.list /etc/apt/sources.list.d/mysql.list

RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update \
  && apt-get install -y \
    mysql-client \
  && apt-get autoremove -yqq --purge \
  && apt-get clean \
  && rm -Rf /var/lib/apt/lists/*

COPY nossl_casebox.conf /etc/apache2/conf-available/casebox.conf
RUN ln -s /etc/apache2/conf-available/casebox.conf /etc/apache2/conf-enabled/casebox.conf

COPY command.sh /command.sh

RUN git clone --depth 1 https://github.com/huridocs/casebox.git /var/www/casebox
COPY config.ini /var/www/casebox/httpsdocs/config.ini
RUN mkdir -p /var/www/casebox/data/tmp/minify
RUN ln -s /dev/stdout /var/www/casebox/logs/ssl_access_log
RUN ln -s /dev/stderr /var/www/casebox/logs/ssl_error_log
RUN ln -s /dev/stderr /var/www/casebox/logs/cb_error_log
RUN chown -R www-data:www-data /var/www
CMD ["/command.sh"]
