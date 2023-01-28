# Bash Arithmetics with Floats Caveats

> ⚠️  There is no official support for floating integers in Bash!


## Tools that you can use in a bash script

1. awk
2. bc
3. almost any other programming language that can be used as a one liner

My favourite by far is `awk` followed by `bc`. Reason being both are commonly
installed on a unix system. However I do prefer `awk` simply because it does
not fail on correctly rounding decimals. Where I found `bc -l` to be a bit more
annoying. If you know your script runs on a system where go, python, perl or
almost any other language is installed on, you could use to solve floats.
However that is not always the case.

Example:

```bash
declare -i x=1
declare -i y=3
##1. kinda works:
bc -l <<< "scale=2; $x/$y"
##2. doesn't work:
bc -l <<< "scale=2; ($x/$y)*100"
```

Output 1: .33 (0.33 preferable)
Output 2: 33.00 (should be 33.33)

Now you can use another tool like `printf` to get rid of the Output 2 rounding
problem. But that just adds complexity for no reason. It would look like this
and even requires a subshell: 

* `printf "%.2f\n" "$(bc -l <<< "($x/$y)*100")"`

```bash
with sprintf awk built-in:
awk '{print sprintf("%.2f", $1/$2*100)}' <<< "$x $y"
with printf:
awk '{printf "%.2f \n", $1/$2*100}' <<< "$x $y"
```

## Caveats

Do not try to attempt to store your result in an as integer defined variable in
a Bash script. Bash does not support floats! Now it sounds obvious but I'm just
saying.

```bash
declare -i x=1
declare -i y=3
declare -i z

## This will fail you:
z=$(awk '{print sprintf("%.2f", $1/$2*100)}' <<< "$x $y")

## This works
declare z
z="$(awk '{print sprintf("%.2f", $1/$2*100)}' <<< "$x $y")"
```

Output 1:  syntax error: invalid arithmetic operator (error token is ".33")

Tags:

    #linux #bash #math #arithmetics #decimals #rounding
