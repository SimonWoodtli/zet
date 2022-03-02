# Bash behavior with functions and arguments/parameters

Basic idea:  
Inside a function, \$1 \.\.\. \$n are the parameters passed to that
function. Outside a function \$1 \.\.\. \$n are the parameters passed to the 
script. So the parameters passed to a script are not the same as parameters
used inside functions. The parameters passed to the script are used for 
function calls or to assign global variables. But then the local variables 
parameters are only active while the function is being called. After that they
reset.

I was thinking that this script would work with hello having \$2 as a local 
variable. But that's not the case since the function call `hello $2` takes "Hey"
but in the body of `hello()` we use \$1 again. As after `greet $1` ran \$1 gets
reset (hence local). 

```bash
#!/bin/bash

greet() {
  local name="$1"
  echo "Hello, $name"
}
greet $1

hello() {
  local greet="$1"
  echo $greet, John
}
hello $2
```

run: `./script xnasero Hey`

If \$2 was used in the body of `hello()` it would need a function call with
two arguments like `hello "hardcoded-arg" $2`.

```bash
#!/bin/bash
greet() {
  local name="$1"
  echo "Hello, $name"
}
greet $1
hello() {
  local greet="$2"
  local friendly="$1"
  echo $greet, $friendly John
}
hello "mister" $2
```
