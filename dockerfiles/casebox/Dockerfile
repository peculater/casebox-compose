FROM php:7.1-apache

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
COPY Ext.ux.plugins.IconCombo.js /var/www/casebox/httpsdocs/js/ux/Ext.ux.plugins.IconCombo.js

# you know what the actual better answer here is?  XSLT.  Don't tempt me.
RUN sed -i 's/enablePositionIncrements="true"//g' /var/www/casebox/sys/solr_configsets/log_config/conf/cb_solr_types.xml \
  && sed -i 's/enablePositionIncrements="true"//g' /var/www/casebox/install/solr/conf/cb_solr_types.xml \
  && sed -i '/defaultSearchField/d' /var/www/casebox/sys/solr_configsets/log_config/conf/schema.xml \
  && sed -i '/defaultSearchField/d' /var/www/casebox/sys/solr_configsets/default_config/conf/schema.xml \
  && sed -i '/defaultSearchField/d' /var/www/casebox/install/solr/conf/schema.xml \
  && sed -i '/lst name="defaults"/a <str name="df">id</str>' /var/www/casebox/sys/solr_configsets/log_config/conf/solrconfig.xml \
  && sed -i '/lst name="defaults"/a <str name="df">content</str>' /var/www/casebox/sys/solr_configsets/default_config/conf/solrconfig.xml \
  && sed -i '/lst name="defaults"/a <str name="df">content</str>' /var/www/casebox/install/solr/conf/solrconfig.xml \
  && sed -i '/defaultOperator/d' /var/www/casebox/sys/solr_configsets/log_config/conf/schema.xml \
  && sed -i '/defaultOperator/d' /var/www/casebox/sys/solr_configsets/default_config/conf/schema.xml \
  && sed -i '/defaultOperator/d' /var/www/casebox/install/solr/conf/schema.xml
COPY config.ini /var/www/casebox/httpsdocs/config.ini
RUN mkdir -p /var/www/casebox/data/tmp/minify
RUN ln -s /dev/stdout /var/www/casebox/logs/ssl_access_log \
  && ln -s /dev/stderr /var/www/casebox/logs/ssl_error_log \
  && ln -s /dev/stderr /var/www/casebox/logs/cb_error_log
RUN chown -R www-data:www-data /var/www
CMD ["/command.sh"]

