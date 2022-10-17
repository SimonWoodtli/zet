# Bash Scripting Caveats: Passing local variables from one function to another

The short answer to this lies in the execution flow. Local variables can only be passed to another function if the function that contains the local variable calls the other function. This will create a parent-child relationship in the shells process system that will allow sharing this variable because the whole environment gets passed to the child.

> Hint: This idea of who executes who is true for the entire shell environment. Procceses that get invoked share the environment from their parents. On the other hand they can never give anything back.

## Example which shares variables

```bash
function1() {
  local foo="🍬🚲"
  ##You can only have my sugar bike if I feed it to you!
  function2 ## I just gave it to you, happy now?
}
function2() {
  ##How can I have this lovely foo variable?
}
```

## Example which won't share local variables

Since function2 invokes function1, function1 becomes a child of function2. A child gets it's environment from the parent hence no candy.

```bash
function1() {
  local foo="🍬🚲"
  ##You can only have my sugar bike if I feed it to you!
}
function2() {
  ##How can I have this lovely foo variable?
  function1
  echo $foo ##Nooo where my sugar at!
}
```

Related: 

* <https://stackoverflow.com/questions/36516512/local-variable-scope-when-one-function-calls-another-inside-bash-shell>

Tags:

    #linux #bash #shell #environment #process #parentChildRelationship #localVariable #function #shareVariable
