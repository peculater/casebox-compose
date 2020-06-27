# Docker-compose for casebox

This works fine, as best I can tell.

## Installation

1. `docker-compose up`
2. In the web container, run 
  `docker-compose exec web /bin/bash -c 'php /var/www/casebox/bin/install.php'`.  The defaults should work for you.  The default mysql password is `terriblepassword`.  Make a note of the name of the "core" that you create at the end there.
2a.  When it asks if you want to reindex your new core, hop into the mysql container and run `GRANT ALL PRIVILEGES ON cb_name-of-the-core.* to 'casebox';`.  Then say Yes to reindexing the core.
3. Head to http://127.0.0.1:8000/name-of-the-core-from-step-2 .  Login is "root" with a password you set in step 2.

## For to make it usable outside of local dev
1.  Update ServerName in nossl_casebox.conf to the externally visible hostname of your instance.  Throw some ServerAliases in there if needed.
2.  Update the database passwords to something not terrible.

## Not done
1.  Email.  Almost certainly will error.
2.  Cron.  I haven't tried to get that going.
3.  Office document parsing.  OpenOffice is installed, but I don't know a good way to test that.
