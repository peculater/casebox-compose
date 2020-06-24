FROM solr:8

# Screw it, run as root.
USER 0
RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update \
  && apt-get install -y \
    git \
  && apt-get autoremove -yqq --purge \
  && apt-get clean \
  && rm -Rf /var/lib/apt/lists/*

RUN mkdir /var/www
RUN git clone --depth 1 https://github.com/huridocs/casebox.git /var/www/casebox

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
RUN chown -R solr:solr /var/www

