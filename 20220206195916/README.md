# User management: $ postgres

## Commands while running postgres (psql)

1. to list all existing users: \du
1. to create user: CREATE USER user_name CREATEDB (creates user and allows him to CREATEDB)
1. to create user with password (I guess for clients with mac/windows): CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
(because linux users login with their shell-login_name) ASK ROB!
1. to switch user permissions: ALTER USER user_name WITH SUPERUSER; (gives super user perm)
1. to give user access to database: grant all privileges on database <dbname> to <username>;

now the chosen user can simply use psql database_name to login directly (no need to sudo su - postgres)
and new database will be created under his name

Tags:

    #postgresql #sql #rdbms #database #administration
