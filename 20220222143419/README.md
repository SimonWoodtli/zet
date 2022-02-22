# bboost21: Day 1-4

## Avoid subshells

* if you want the whole path:  
`foo=/some/bar; echo ${foo}`

* if you only want the filename:
without subshell:  
`foo=/some/bar; echo ${foo##*/}`

* with subshell:  
`foo=/some/bar; basename $foo`

## Avoid vimscript less plugins is more

Make use of vim's tight integration with the shell. Instead of using yet 
another plugin to comment code you can write a filter in this case `cmt`. Which
can be called within vim with the double bang.

The standard comment cmt uses is \#
1. In vi-mode go to line you want commented:
2. `!!cmt`
3. If you want another comment type: (e.g. //) `!!cmt //`

### General syntax/usage for double bang commands

!!ComandName 

The command can be a bash builtin function, linux command line program, 
own script, heck even a nodejs script python script or whatever you fancy.

```markdown
!} for current paragraph
!G for whole document
!! for current line
```

### Some examples of double bang usage:

Math in vim:
1. write this line in vim: `scale=2; 3*45/1.2`
2. execute bc on the line you just wrote: `!!bc`

Bash commands in vim:
1. write this line in vim: `for i in {00..100}; do echo \$i; done`
2. execute bash on the line you just wrote: `!!bash`

Convert markdown to html in vim:
1. write your markdown e.g.
2. execute pandoc on the very first line: `!Gpandoc`

```markdown
# hello world

Foo|Bar
-|-
3|5
2|1
```

Linter in vim:
1. write your php or whatever code
2. execute a linter on the very first line: `!!Gphp8 -l %`

Tags:

    #linux #vim #shell #bash #linter #convert
