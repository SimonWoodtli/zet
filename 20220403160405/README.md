# sed commands

```sh
#!/bin/sh
# see also `man sed` and `info sed`

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## SED
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# sed (1) - stream editor for filtering and transforming text

# print 3 lines of text
printf 'cat\ndog\ntree'
# cat
# dog
# tree

## q command

# execute a sed command (explicit)
printf 'cat\ndog\ntree' | sed -e 'q'
# cat

# execute a sed command (implicit)
printf 'cat\ndog\ntree' | sed 'q'
# cat

# quit at 2nd line
printf 'cat\ndog\ntree' | sed '2q'
# cat
# dog

# quit at line with 'do' pattern
printf 'cat\ndog\ntree' | sed '/do/q'
# cat
# dog

## p command

# print each line (and automatic printing)
printf 'cat\ndog\ntree' | sed 'p'
# cat
# cat
# dog
# dog
# tree
# tree

# print 2nd line (and automatic printing)
printf 'cat\ndog\ntree' | sed '2p'
# cat
# dog
# dog
# tree

# suppress automatic printing and print each line
printf 'cat\ndog\ntree' | sed -n 'p'
# cat
# dog
# tree

# print 2nd line
printf 'cat\ndog\ntree' | sed -n '2p'
# dog

# print from 2nd line to 3rd line
printf 'cat\ndog\ntree' | sed -n '2,3p'
# dog
# tree

# print from 2nd line to end line
printf 'cat\ndog\ntree' | sed -n '2,$p'
# dog
# tree

# print end line
printf 'cat\ndog\ntree' | sed -n '$p'
# tree

# print from 'dog' pattern to 3rd line
printf 'cat\ndog\ntree' | sed -n '/dog/,3p'
# dog
# tree

# print from 'dog' pattern to 'tree' pattern
printf 'cat\ndog\ntree' | sed -n '/dog/,/tree/p'
# dog
# tree

# print each line and quit at 2nd line
printf 'cat\ndog\ntree' | sed -n 'p;2q'
# cat
# dog

# print and quit at 2nd line
printf 'cat\ndog\ntree' | sed -n '2{p;q}'
# dog

## d command

# delete 2nd line
printf 'cat\ndog\ntree' | sed '2d'
# cat
# tree

# delete from 2nd line to 3rd line
printf 'cat\ndog\ntree' | sed '2,3d'
# cat

# print from 1st line to 2nd line and delete from 2nd to 3rd line
printf 'cat\ndog\ntree' | sed -n '1,2p;2,3d'
# cat
# dog

# delete from 2nd to 3rd line and print from 1st to 2nd line
printf 'cat\ndog\ntree' | sed -n '2,3d;1,2p'
# cat

## s command (most common)

# substitute 'dog' pattern with 'DOG' pattern
printf 'cat\ndog\ntree' | sed 's/dog/DOG/'
# cat
# DOG
# tree

# substitute 'dog' pattern with 'DOG' pattern (using '_' as separator)
printf 'cat\ndog\ntree' | sed 's_dog_DOG_'
# cat
# DOG
# tree

# substitute 'dog' pattern with 'DOG' pattern (within a line)
printf 'cat dog\ndog\ntree' | sed 's/dog/DOG/'
# cat DOG
# DOG
# tree

# substitute 'dog' pattern with 'DOG' pattern (only first if there are multiple)
printf 'cat\ndog dog\ntree' | sed 's/dog/DOG/'
# cat
# DOG dog
# tree

# globally substitute 'dog' pattern with 'DOG' pattern
printf 'cat\ndog dog\ntree' | sed 's/dog/DOG/g'
# cat
# DOG DOG
# tree

# substitute 2nd 'dog' pattern with 'DOG' pattern
printf 'cat\ndog dog dog\ntree' | sed 's/dog/DOG/2'
# cat
# dog DOG dog
# tree

# substitute 'dog' pattern with 'DOG' pattern and print lines
printf 'cat\ndog dog\ntree' | sed 's/dog/DOG/p'
# cat
# DOG dog
# DOG dog
# tree

# substitute 'dog' pattern with 'DOG' pattern and write lines to '/tmp/out.txt'
printf 'cat\ndog dog\ntree' | sed 's/dog/DOG/w /tmp/out.txt'
# cat
# DOG dog
# tree

# print content of '/tmp/out.txt'
cat /tmp/out.txt
# DOG dog

# substitute 'ls' pattern with 'ls -lh' pattern and execute lines
printf 'ls /bin/echo' | sed 's/ls/ls -lh/e'
# -rwxr-xr-x 1 root root 31K Feb 18  2016 /bin/echo

# remove trailing spaces
printf 'hello world   	' | sed 's/\s\+$//' && echo '|'
# hello world|

# remove empty lines
printf 'cat\n	 \ntree' | sed '/^\s*$/d'
# cat
# tree
```

Related:

• <https://gokcehan.github.io/>

Tags:

    #linux #sed #commands #cheatsheet
