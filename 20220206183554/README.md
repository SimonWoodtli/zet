# MongoDB shell commands

* To check for your current database: `db`
* To create a new database: `use database_name`

* To create an array use the db global variable followed by a dot and the presidents property/collection another dot and the insertMany method: `db.collection_name.insertMany([])`
* basic structure: global/local_variable.property/collection.method/function()


HINT: Mongo shell uses javascript. Hence we can use 'George' or "George".

examples of regular expressions in mongoDB:

```
WHERE "col" LIKE 'begin%' becomes { col: /^begin/ }
WHERE "col" LIKE '%middle%' becomes { col: /middle/ }
WHERE "col" LIKE 'end%' becomes { col: /end$/ }
WHERE LOWER("col") LIKE '%case insensitive%' becomes { col: /case insensitive/i } (note the i after the closing /)
```

// Queries
* to make them look nice: `db.collection_name.find({}).pretty()`
* select * from table_name; use: `db.collection_name.find({})`
* select * from table_name where firstName = 'George'; use:  
`db.collection_name.find({
  firstName: 'George'
})`

* select * from table_name where firstName != 'George'; use:
one line:  
`{ firstName: { $not: { $eq: 'John' } } }` == `WHERE "first_name" != 'John'`
`db.collection_name.find({
  firstName: {
    $not: { $eq: 'George' }
    }
})`

* select * from table_name where firstName = 'George' AND lastName = 'Washington'; use:  
`db.collection_name.find({
  firstName = 'George',
  lastName = 'Washington'
})`

* select * from table_name where lastName = 'Washington' OR lastName = 'Jefferson'; use:  
`db.collection_name.find({
  $or: [
      { lastName: 'Washington' },
      { lastName: 'Jefferson' }
  ]
})`

* // /ash/ = syntax for regular expressions
select * from table_name where lastName LIKE '%ash%';  
`db.collection_name.find({
  lastName: /ash/
})`

* select * from table_name where lower(lastName) LIKE 'wash%';  
`db.collection_name.find({
  lastName: /^wash/i
})`

* delete from table_name where id = '202';  
`db.collection_name.deleteOne({ _id: '202'})`  
OR: `db.collection_name.deleteMany({ _id: '202'})`

Related:



Tags:

    #mongoDB #NoSQL #commands #mongo-shell #terminal
