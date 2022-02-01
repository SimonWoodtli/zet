# postgresql setup to use pgadmin4

check status: `service postgresql status`
or `ss -nlt` (look for 127.0.0.1:5432)

To delete a user eg. 'user_2': `DROP USER user_2;`

1. change user: `sudo su postgres`
1. use psql tool: `psql`
1. check for current databases: `\l`
1. check for users: `\du`
1. change password of default root_user 'postgres': `ALTER USER postgres WITH PASSWORD 'zt^#*4LsLwg!q&#BvuFzedBc8#PYXk';`
1. create new user (don't create databases with default root user): `CREATE USER sero WITH PASSWORD 'quader88';`
1. Check if user created: `\du` its listed but no priviliges added to it
1. make sero superuser: `ALTER USER sero WITH SUPERUSER;`

Now you can use host: localhost user: user_1 and your password to connect to server in pgadmin4


## create database

1. change user: `sudo su postgres` (i need to find a way to switch to the user not use postgres acc)
1. use psql tool: `psql` (this should be done without using postgres acc, rather in my account sero, maybe I can just change the owner after creating database??)
1. create database `create database test`

## connect database

`\c name_of_database`

Tags:

    #postgresql #linux #server #database
