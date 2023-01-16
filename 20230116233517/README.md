# Difference between `<`, `<<` and `<<<` operators in Bash 

> ⚠️  I don't mean the `<` that is used in test conditions `[..<..]` or numeric
conditions `((..<..))`.

So I mean the input redirection operator (`<`). Which is used for reading from
a file and sending the output to a command like so `python hello.py < foo.txt`.
This would feed foo.txt to the executing python script.

1. Input redirection: `command < file` = means exec command but read
   information from file
2. Here string: `command <<< string`  | `command <<< $variable` = the first one
   means literally read the word string into the command and the second one
   could be used in a script to read value from a variable
3. Here doc:

```bash
cat <<END
multi line 
content
END
```

> 📝 Here 'END' is used as a delimiter but any delimiter can be used 'EOF' or
'whatever' it just marks the start end end of reading. By convention usually
'EOF' is used.

## Names of operators

* < = input redirection operator
* << = here document operator
* <<< = here string operator

Tags:

    #linux #bash #commands #explained #operators
