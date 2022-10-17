# Bash Scripting Caveats: print multiline variables

If you have a variable which stores a string that is multiple lines long. The `echo` might not behave as you wish and prints the whole variable on a single line. You can use `echo` however make sure to quote the variable. 

* wrong: `echo $foo`
* right: `echo "$foo"`

> Hint: Remeber we should always quote variables if we use them in commands. Sometimes, using shell variables without quoting can be dangerous.

## Why unquoted variable won't work?

The shell reads the value of the \$foo variable and splits it into arguments, using the \$IFS env. variable to feed the `echo` command.
The \$IFS by default splits at any whitespace that includes spaces, tabs and new lines.

## Why quoted variable will work?

The shell treats quoted variables as one single argument. Hence it won't split.

### Additional Way

* here-string: `cat <<<$foo` (works even unquoted)

Related:

* <https://www.baeldung.com/linux/variable-preserve-linebreaks>

Tags:

    #bash #linux #shell #variables #sto #echo
