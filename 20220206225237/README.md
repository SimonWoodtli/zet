# JSON data Postgres

In postgres there are two json data types, but jsonb is more efficient, so you usually want to use that one.

create some jSON:

```sql
CREATE TABLE "json_test" (
  "values_jsb" JSONB
);

INSERT INTO "json_test" VALUES
('{"name": "Alice", "age": 30}'),
('{"name": "Bob", "language": "English"}');
```

Query json:

```sql
SELECT "values_jsb"->>'name' FROM "json_test";
SELECT "values_jsb" FROM "json_test" WHERE "values_jsb"->>'name' = 'Alice';
```

Postgres offers many data types that we won't have the time to explore in this course. Here, we've seen an example of that with the JSONB type, which allows us to store composite values inside a single column. Care should be taken when using this type in  order to make sure we don't negate the benefits of normalized data.

Tags:

    #postgresql #rdbms #administration #sql #json
