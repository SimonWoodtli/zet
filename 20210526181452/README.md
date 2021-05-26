# bboost20, JSON, YAML, Structured Data

##  Wednesday, June 10, 2020, 7:15:15PM

### JSON

***HINTS***
* where used? anything that has to be read or exchanged with the internet. e.g. API from web
* `jq` is the best bet to deal with JSON
* JSON is the universal standard for exchanging data between applications.
* needs double quotes for strings and variables. single don't work
* left side is string right side is variable
* nesting is possible for lists

`curl https://api.github.com` shows ugly list
`curl https://api.github.com | jq .` shows list it better structured because of the dot= select entire content of json file and print it out nice.
`curl https://api.github.com | jq . > /tmp/github.json`


1.types: 1, "something", 1.2, 1.2e10, -1.2, null (empty string= no value), true, false (and you can combine them together)
1. [1,34,-10,34e10,"something"] is  a list with numbers and a string
1. list is also called array or collection. list is python term for it
1. {"age":52} is an object (json object notation), go calls these maps, associative array and dictionary is another word
1. {
      "age":52,
      "name": "Rob"
   }


### YAML

it's a structure data language that is easy to read for humans unlike JSON.

### JQ tool

***HINTS***
* not the fastest for parsing
* a * in yaml means expand this as if all the whole line is written (it helps to just write endings of things) keyword: dereference (a pointer to a thing to which you can later reference to)

1. `jq .age /tmp/types.json` will output your 52 (see JSON)
----
### General Notes
* bbb
#### Common Practice
1. bb
#### Tips
1. bb
