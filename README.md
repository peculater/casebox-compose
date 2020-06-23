# Docker-compose for casebox

This works....eh.

## Installation

1. `docker-compose up`
2. In the solr container, run `docker-compose exec solr /bin/bash -c 'mkdir /var/solr/data/configsets/; cp -r /opt/solr/server/solr/configsets/_default /var/solr/data/configsets/cb_default'`
3. In the web container, run 
  `docker-compose exec web /bin/bash -c 'php /var/www/casebox/bin/install.php'`.  The defaults should work for you.  The default mysql password is `terriblepassword`.  Make a note of the name of the "core" that you create at the end there.
3a.  It will complain that it can't find the solr directory on disk, that's fine.  You dealt with that in step 2.
3b.  When it asks if you want to reindex your new core, hop into the mysql container and run `GRANT ALL PRIVILEGES ON cb_name-of-the-core.* to 'casebox';`.  Then say Yes to reindexing the core.
4. Head to http://127.0.0.1:8000/name-of-the-core-from-step-3 .  Login is "root" with a password you set in step 3.

