# bboost20: Day 17

Linux Beginner Boost - Day 17 (May 26, 2020)

##  Wednesday, May 26, 2020, 01:28:49PM

### Lesson 1 Variables

***HINTS***
* path export in your bashrc means that children processes of the shell will have that exported variable.
* echo works with strings and hear documents and hear strings work with pipes.
* always use "" when you do echo in a script `echo "$name"`


#### Command Substitution

* don't use backticks, they might be easier to type but $ is able to nest
they are useless.
* need \$(...)

see `greet2`
run a command in a command= command substitution
echo "Today is \$(date)"

#### Arithmetic Expansion

* beware of floating point arithmetic (zsh), bash does not provide it
* use `bc` in shell for floating points, it is much better than echo as your default calculator. e.g. `echo "scale=2; 3/2" | bc` is less elegant than go for this: `bc <<< "scale=2; 3/2"` (very accurate works everywhere)
* hear strings are better than loops because variable expansion work better.
need \$((...))
see `greet3`
echo "Result: \$((2 * (RANDOM%age))"

#### Functions, Loops

* a function is just a "mini script", writing commands in the shell is programming!
* they are both functions, however pure functions and procedural functions are not the same. refer to procedures is this dock from here on
* procedure is imperative programming
* pure functions and procedures are not the same, if you understand the difference you will write better code. Because you will not mix the two. A function returns something that is critical. A procedure with side effects and subroutines and stuff like that does things to your environment.
* a pure function is like in math it takes input and gives output
* a procedure has other effects on it's environment as well.
* It is harder to test a procedure than a pure functions
* If you are reading any numbers from the command line in any way you always have to parse out the string and convert it into a number.
What is the difference between an argument and a parameter? (computer science only, in math it is the opposite)
f(x) = 2x + 3
f(2) = (2*2) + 3
x: parameter
2: argument

see `greet5` example of a procedure/subroutine, it is like do this now do that, a bunch of actions together. why? because it is doing something outside the scope of itself. It is accessing the terminal and spitting some content out.

#### JSON Types

* the best way to learn types is to learn JSON, before you even learn programming.
* Data structure languages are very important: They are how you capture and share data.
* JSON is a data language beyond just javascript.
* YAML is backward compatible with JSON. Why use it? It is easier to type than JSON.
* All computer languages use some form of a data structure. JSON is one of those data structures. XML is another one. However you only need XML for SVG picture animation, otherwise it sucks. Third one is YAML. Any JSON is also valid YAML but YAML adds more things to it that are specific to humans. YAML was made for humans to edit. JSON was made for computers to edit.
* Key:Value golden rule for JSON and add , @end of line
* JSON is the language for communication between everything. Very important!
* And with API nowadays they (api) talk to webservers but they don't get HTML they get JSON from webservers. So they can change things.
* JSON is builtin to GO language
* unmarshalling is when you bring data from a flat file like the example JSON example below into a data structure that is running inside of a program. marshalling is the opposite when you write it out of the data structure to the flat file.
* JSON is used for marshalling/unmarshalling and the transport of that file is text.
* data types: strings, boolean, numbers. Everything else in JSON is a collection of these data types.
* two collections in JSON: objects and arrays.
* `jq` command in the shell lets you run jSON files, or parts of it whatever you like.
* in JSON line return need \n which is quite annoying. YAML does not need that (doesn't care about white space).
* YAML can be converted to JSON!
* You can use YAML as a database because it can reference (like with a symbolic link) other data structures.

* You can use YAML as a database because it can reference (like with a symbolic link) other data structures.
\# a string is a series of letters, numbers emoji whatever that are bounded with quotes
\# 14.4 in math is a decimal, in computer science it is a floating number. Doubles have a larger size and more precision than floats.
\# true/false are called booleans
\# the stricter the language the better you specify what type it is
\# in JSON a collection can be an object in python its dictionaries (Historic term is map or hashmaps).
\# Another collection is an array or list in python e.g. [10, 14.4].
\# "pie": "Apple", (this is a key colon value combination, pie is the name Apple value)
\# to show a collection use {content}
\# you can also combine numbers with names or booleans e.g. "likes": true or "numbers": [10, 14.4]
`touch languages.json`
`vi languages.json`
{"Apple"

6.673e-11
10
14.4

true
}

----

### General Notes

* bbb

#### Common Practice

1. bb

#### Tips

1. bb
