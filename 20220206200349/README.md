# Create Postgres database

```sql
CREATE DATABASE name_of_database
  WITH
  OWNER = postgres (or any other superuser)
  ENCODING = 'UTF8'
  CONNECTION LIMIT = -1;
```

or difference??
`postgres=# create database dvdrental;`

Tags:

    #postgresql #rdbms #sql #database
