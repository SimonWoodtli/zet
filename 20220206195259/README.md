# Connect database with specific user

connect to postgres on port 5432 on database postgres as username


if no \-\-username used: the active user in the current shell session will be used!
option 1: while starting psql: `psql -p5432 -d "postgres" --username=username`  
`psql -p5432 -d "postgres" -U username`

option 2: while in psql: `\c name_of_database` or `\c name_db username`

hint: the username issue should resolve itself, once you setup postgres to allow a specific user to connect to `psql`. (see Related)

Related:

* []() blabla

Tags:

    #postgresql #rdbms #sql #database
