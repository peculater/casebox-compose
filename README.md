# Docker-compose for casebox

This works....eh.

## Installation

1. `docker-compose up`
2. In the web container, run 
  `docker-compose exec web /bin/bash -c 'php /var/www/casebox/bin/install.php'`.  The defaults should work for you.  The default mysql password is `terriblepassword`.  Make a note of the name of the "core" that you create at the end there.
2a.  When it asks if you want to reindex your new core, hop into the mysql container and run `GRANT ALL PRIVILEGES ON cb_name-of-the-core.* to 'casebox';`.  Then say Yes to reindexing the core.
3. Head to http://127.0.0.1:8000/name-of-the-core-from-step-3 .  Login is "root" with a password you set in step 3.

