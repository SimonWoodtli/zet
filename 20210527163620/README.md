# What are some basic psql commands?

* run service: `service postgresql start`
* use linux-commands: `\! shell-command`
* to see each column per line: `\x`
* psql-shell help: `\?` or `\help`
* linux-shell: `man postgres`, `man psql`, `psql --help`
* see time for query: `\timing on`
* turns autocommit of (if you want to control transaction manually):  `\set AUTOCOMMIT off` followed by `BEGIN;` do your query or DDL/DML then `COMMIT;` if you don't like your changes: `ROLLBACK` aborts the whole transaction
* TRUNCATE: used to erase content of a table. But only the rows or "content": `TRUNCATE TABLE tbl_name;`
* to delete and restart the serial (id) too: `TRUNCATE TABLE tbl_name RESTART IDENTITY;`

## comments

* COMMENT: used to add comments for other programmers that work on the database `COMMENT ON COLUMN tbl_name.column_name IS 'here you write your comment';`
* to see comments on columns: `\d+ tbl_name`

## tables:

* a list of the currents database tables: `\d` or `dt` if you only wanna see tables
* details (all columns) of a table `\d table_name`
* `TABLE tb_name;` shortcut for `select * from tb_name`

think of \d as d for describe!

## database:

* list all databases: `\l`
* create database:  `CREATE DATABASE dbname;`
* delete database: `DROP DATABASE dbname;`


## users:

* list all users: `\du`
* create a user: `CREATE USER uname;` or with additional privileges: `CREATE USER sero SUPERUSER;`
* privileges: SUPERUSER, CREATEDB, CREATEUSER, IN GROUP, (ENCRYPTED) PASSWORD 'password', VALID UNTIL, SYSID uid

Hint: for linux users create a username they already have on the postgres-server. This allows to directly use `psql dbname`

## time:

* to see current timezone set to postgres: `SHOW TIMEZONE;`
* `SELECT CURRENT_TIMESTAMP;`,
`SELECT CURRENT_DATE;` = `SELECT CURRENT_TIMESTAMP::DATE`
* change timezone of postgres-server: `SET TIMEZONE='America/Los_Angeles';`
