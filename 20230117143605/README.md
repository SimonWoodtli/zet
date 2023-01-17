# Test if variable is an array in Bash

A great function to test whether or not your var is an array

```bash
array_test() {                                                                  
    # no argument passed                                                        
    [[ $# -ne 1 ]] && echo 'Supply a variable name as an argument'>&2 && return 2
    local var=$1                                                                
    # use a variable to avoid having to escape spaces                           
    local regex="^declare -[aA] ${var}(=|$)"                                    
    [[ $(declare -p "$var" 2> /dev/null) =~ $regex ]] && return 0               
}

array_test myvar && echo it's an array
```

Related:

* <https://stackoverflow.com/questions/14525296/how-do-i-check-if-variable-is-an-array>

Tags:

    #linux #bash #array #test
