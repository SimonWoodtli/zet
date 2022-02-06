# Import data Postgres

## import backup

1. psql-shell create database dbname: `CREATE DATABASE dbname;`
1. linux-shell: `psql -U username dbname < dbexport.pgsql` (from a datadump-file see `pg_dump`, 20200926182615)

## import .sql or data_dump backup

Create random data on https://mockaroo.com/ first and download it as sql with a create statement. Once downloaded you can adjust the create statement to your own needs!

You can have .sql with only 'insert row statements' in it. Or with a 'create table statement' at the top (then you dont't have to create the table upfront)

1. psql-shell: first `psql database_name` then either create a table first or have the create table statement in your .sql: IMPORT: `\i /place/of/file/file.sql`
2. linux-shell: `psql -U username dbname < file.sql`


## import .csv

PostgreSQL requires the superuser privilege to access files on the database server for security reasons. To allow normal users: use the linux-shell 'COPY tabname FROM STDIN' via pipe.

1. create database and table with the correct datatypes that are listed in same order as in your .csv
2. superuser: `COPY table_name FROM '/home/user/file.csv' DELIMITER ',' CSV HEADER;`
3. normal user:`cat myfile.csv | psql -d mydb -c "COPY landing_tbl(field01, field02...) FROM STDIN CSV;"`

## import .tsv

1. create database and table with the correct datatypes that are listed in same order as in your .tsv
2. superuser: `COPY table_name FROM '/home/xna/file.tsv' DELIMITER E'\t';`
3. normal user: `cat result.tsv | psql -d db_name -c "COPY tbl_name(column1, column2 ...) FROM STDIN;"`

## import json

each key on its own column: is there even a situation where you would want that? ask rob?????

https://stackoverflow.com/questions/39224382/how-can-i-import-a-json-file-into-postgresql
https://stackoverflow.com/questions/17807030/how-to-create-index-on-json-field-in-postgres
https://kb.objectrocket.com/postgresql/how-to-import-a-json-file-into-postgresql-database-cluster-1437
https://info.crunchydata.com/blog/fast-csv-and-json-ingestion-in-postgresql-with-copy

multi line:
single line array:

json data in a single column (column needs to be json datatype):
[checkout](https://www.postgresql.org/docs/current/app-psql.html#APP-PSQL-INTERPOLATION)

* to handle json data in postgres and do queries (see ... another tutorial)


## import yaml

not supported, only via extension can the whole schema (data_dump) be imported:
https://pgxn.org/dist/pyrseas/docs/yamltodb.html

Tags:

    #sql #postgresql #rdbms #database #administration
