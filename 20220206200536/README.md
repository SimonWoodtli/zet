# Export data Postgres

\\copy allows non superusers to copy data too! unlike `COPY ...` statements

### Single database

• important: you probably don’t want to export the id/uid and dump them (security issue). Find a way to exclude that!

from server linux-shell: pg_dump -U username dbname > dbexport.pgsql

example: as tar
pg_dump -U xnasero -W -F t dvdrental > /home/xna/dvdrental.tar

### All databases

pg_dumpall -U xnasero > /home/xna/all.sql

## Export data to .csv

• if you want no headers don’t add it at end


### Export with conditions/joins

\copy (SELECT * FROM table_name) TO '/home/username/filename.csv' DELIMITER ',' CSV HEADER;

### Export whole table

\copy table_name TO '/home/username/filename.csv' DELIMITER ',' CSV HEADER;

if you want csv with a semicolon delimiter simply change ',’ to ';’

## Export data to .tsv

• if you want .tsv you can’t export header

\copy (SELECT * FROM table_name) TO '/home/username/filename.csv' DELIMITER E'\t';
\copy table_name TO '/home/xna/test.tsv' DELIMITER E'\t';

## Export data to .sql

I guess same as like pg_dump backup yeah it is

## Export json

copy all or some columns to multiple rows:
\copy (SELECT row_to_json(row(sub)) FROM (SELECT * FROM table_name) sub) TO '/home/xna/filename.json'

copy all columns to multiple rows:
`\copy (SELECT row_to_json(table_name) FROM table_name) TO '/home/xna/filename.json'`

copy all columns to single row in brackets! (what you want):
\copy (SELECT array_to_json(array_agg(row_to_json(sub))) FROM (SELECT * FROM table_name) sub) TO '/home/xna/filename.json'


### Export json: queries

This statement will query some columns and preserves the number of rows:
SELECT row_to_json(sub)
  FROM (SELECT column1, column2 FROM table_name) sub;

This statement will query all columns in the table and the preservers the number of rows:
SELECT row_to_json(table_name) FROM table_name;

This statement will query all columns but put all rows into a single line (more json style):
SELECT array_to_json(array_agg(row_to_json(sub)))
  FROM (SELECT * FROM table_name) sub;

## Export yaml

only a data_dump possible with whole schema: (not integrated needs pgxn extension)
https://pgxn.org/dist/pyrseas/docs/dbtoyaml.html

if only data without schema wanted: export as json and then convert it to yaml (`json2yaml filename.json`)

Tags:

    #postgresql #sql #database #administration #rdbms
