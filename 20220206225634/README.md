# Setup Postgres Server

## Basics

1. Go to their [website] and follow the installation.
1. `sudo su - postgres`
1. `service postgresql start` //you can add this to startup files, to auto connect
1. `psql`

When postgres gets installed the user ‘postgres’ gets automatically setup. However this user does not come with a password.
If you try to run `psql` as your normal user or even as ‘root’ you will get an error because only ‘postgres’ can access `psql` by default.
-> create any username that already exists on your linux server. That will allow this user to access `psql`

## Configuration

1. check existing users: `\du`
1. create new user: (see Related)
1. create database: `CREATE DATABASE db_name`

Related:

* [20220206195916](/20220206195916/) User management: $ postgres

Tags:

    #postgresql #sql #rdbms

[website]: <https://www.postgresql.org/download/linux/>
