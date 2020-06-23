# Docker-compose for casebox

This works....eh.

## Installation

1. `docker-compose up`
2. In the solr container, run `mkdir /var/solr/data/configsets/; cp -r /opt/solr/server/solr/configsets/_default /var/solr/data/configsets/cb_default'`
3. In the web container, run `php /var/casebox/bin/install.php`.  The defaults should work for you.  The default mysql password is `terriblepassword`.  Make a note of the name of the "core" that you create at the end there.
4. Head to http://127.0.0.1:8000/name-of-the-core-from-step-3 .  Login is "root" with a password you set in step 3.

