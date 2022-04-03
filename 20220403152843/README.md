# Shell expansions and control flows

```sh


#!/bin/sh
# see also `man sh` and `man bash`
# see https://wiki.ubuntu.com/DashAsBinSh for bash specific (non-POSIX) features

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## EXPANSIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# bc (1) - An arbitrary precision calculator language

## tilde expansion

echo ~      # /home/gokcehan
echo ~foo   # ~foo
echo ~/foo  # /home/gokcehan/foo
echo foo~   # foo~
echo foo/~  # foo/~
echo '~'    # ~
echo "~"    # ~

## parameter expansion

foo="hello"

echo $foo       # hello
echo $foobar    # (prints nothing since there is no $foobar)
echo ${foo}bar  # hellobar
echo '$foo'     # $foo
echo "$foo"     # hello

## command substitution

which ls          # /bin/ls
ls -l /bin/ls     # -rwxr-xr-x 1 root root 133792 Oca 18  2018 /bin/ls
ls -l `which ls`  # -rwxr-xr-x 1 root root 133792 Oca 18  2018 /bin/ls

echo `which ls`            # /bin/ls
echo `ls -l \`which ls\``  # -rwxr-xr-x 1 root root 133792 Oca 18  2018 /bin/ls
echo $(which ls)           # /bin/ls
echo $(ls -l $(which ls))  # -rwxr-xr-x 1 root root 133792 Oca 18  2018 /bin/ls
echo '`which ls`'          # `which ls`
echo "`which ls`"          # /bin/ls

## arithmetic expansion

echo 2+2                   # 2+2
echo $((2+2))              # 4
echo $((4/2))              # 2
echo $((5/2))              # 2
echo '5/2' | bc            # 2
echo 'scale=10; 5/2' | bc  # 2.5000000000
echo '5/2' | bc -l         # 2.50000000000000000000

## glob expansion

echo *       # foo foo.c bar bar.c ...
echo *.c     # foo.c bar.c ...
echo *.[ch]  # foo.c bar.h ...
echo *.?     # foo.c bar.h baz.a ...
echo '*'     # *
echo "*"     # *

## brace expansion (bash only -- not POSIX standard)

echo foo.{c,h}    # foo.c foo.h
echo foo{.c,}     # foo.c foo
echo 'foo.{c,h}'  # foo.{c,h}
echo "foo.{c,h}"  # foo.{c,h}

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## CONDITIONALS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# true (1) - do nothing, successfully
# false (1) - do nothing, unsuccessfully
# test (1) - check file types and compare values
# [ (1) - check file types and compare values
# uname (1) - print system information

# run an example command
echo hello world
# hello world

# show the return value of last command (0 is true/success, non-0 is false/fail)
echo $?
# 0

# TIP: you may put the following in your '~/.bashrc' to display error codes
EC() { echo -e '\e[1;33m'code $?'\e[m\n'; }
trap EC ERR

# statement separator ';' is optional at the end of line
echo hello;
# hello

# use ';' to separate multiple statements in a single line
echo hello; echo world
# hello
# world

# run 'true' and show its return value
true; echo $?
# 0

# run 'false' and show its return value
false; echo $?
# 1

# run 'echo' command since first condition is true (short-circuit evaluation)
true && echo 'is true'
# is true

# skip 'echo' command since first condition is false (short-circuit evaluation)
false && echo 'is false'
# (no output)

# run first 'echo' command since first condition is true (short-circuit evaluation)
true && echo 'is true' || echo 'is false'
# is true

# run second 'echo' command since first condition is false (short-circuit evaluation)
false && echo 'is true' || echo 'is false'
# is false

# check whether '/bin' exists
test -e /bin; echo $?
# 0

# check whether '/bin/ls' exists
test -e /bin/ls; echo $?
# 0

# check whether '/bin' exists as a file
test -f /bin; echo $?
# 1

# check whether '/bin/ls' exists as a file
test -f /bin/ls; echo $?
# 0

# check whether '/path/to/non/existent/file' exists as a file
test -f /path/to/non/existent/file; echo $?
# 1

# check whether '/bin' exists as a directory
test -d /bin; echo $?
# 0

# check whether '/bin/ls' exists as a directory
test -d /bin/ls; echo $?
# 1

# check whether '/path/to/non/existent/dir' exists as a directory
test -d /path/to/non/existent/dir; echo $?
# 1

# check whether '/bin/ls' is readable
test -r /bin/ls; echo $?
# 0

# check whether '/bin/ls' is writable
test -w /bin/ls; echo $?
# 1

# check whether '/bin/ls' is readable AND writable
test -r /bin/ls -a -w /bin/ls; echo $?
# 1

# check whether '/bin/ls' is readable OR writable
test -r /bin/ls -o -w /bin/ls; echo $?
# 0

# check whether '/bin/ls' is NOT readable
test ! -r /bin/ls; echo $?
# 1

# check whether 'HOSTNAME' variable is of non-zero length
test -n $HOSTNAME; echo $?
# 0

# check whether 'HOSTNAME' variable is of zero length
test -z $HOSTNAME; echo $?
# 1

# check whether the kernel is 'Linux'
test $(uname -s) = "Linux"; echo $?
# 0

# check whether the machine is 'x86_64'
test $(uname -m) != "x86_64"; echo $?
# 1

# check whether '/bin' exists (alternative syntax)
[ -e /bin ]; echo $?
# 0

# you need a space after '['
[-e /bin ]; echo $?
# /bin/bash: [-e: command not found

# you need a space before ']'
[ -e /bin]; echo $?
# /bin/bash: line 0: [: missing `]'

# you need a space before '='
[ $(uname -m)= "x86_64" ]; echo $?
# /bin/bash: line 0: [: x86_64=: unary operator expected

# you need a space after '='
[ $(uname -m) ="x86_64" ]; echo $?
# /bin/bash: line 0: [: x86_64: unary operator expected

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## CONTROL FLOW
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# basename (1) - strip directory and suffix from filenames
# seq (1) - print a sequence of numbers

## if/elif/else/fi

arch=$(uname -m)

if [ $arch = "x86_64" ]
then
    echo 'system is 64 bit'
fi
# system is 64 bit

if [ $arch = "x86_64" ]; then
    echo 'system is 64 bit'
fi
# system is 64 bit

if [ $arch = "x86_64" ]; then echo 'system is 64 bit'; fi
# system is 64 bit

if [ $arch = "x86_64" ]; then
    echo 'system is 64 bit'
elif [ $arch = "i686" ]; then
    echo 'system is 32 bit'
fi
# system is 64 bit

if [ $arch = "x86_64" ]; then
    echo 'system is 64 bit'
elif [ $arch = "i686" ]; then
    echo 'system is 32 bit'
else
    echo 'system is unknown'
fi
# system is 64 bit

## while/until/done

i=0
while [ $i -lt 5 ]; do
    sleep 1
    i=$((i+1))
    echo "$i second(s) have passed"
done
# 1 second(s) have passed
# 2 second(s) have passed
# 3 second(s) have passed
# 4 second(s) have passed
# 5 second(s) have passed

i=0
until [ $i -ge 5 ]; do
    sleep 1
    i=$((i+1))
    echo "$i second(s) have passed"
done
# 1 second(s) have passed
# 2 second(s) have passed
# 3 second(s) have passed
# 4 second(s) have passed
# 5 second(s) have passed

## for/done

for fruit in apple orange banana; do
    echo $fruit
done
# apple
# orange
# banana

for fruit in 'apple orange banana'; do
    echo $fruit
done
# apple orange banana

for fruit in "apple orange banana"; do
    echo $fruit
done
# apple orange banana

for fruit in 'red apple' 'orange' 'banana'; do
    echo $fruit
done
# red apple
# orange
# banana

for f in *.c; do
    echo $f
done
# foo.c
# bar.c
# baz.c

# parsing 'ls' output can NOT handle whitespaces in filenames
for f in $(ls ~); do
    echo "$f"
done
# cmpe230
# Desktop
# Documents
# Downloads
# examples.desktop
# Music
# Pictures
# Public
# Templates
# Videos
# VirtualBox
# VMs

# use glob expansion instead of 'ls' for safety
for f in $HOME/*; do
    echo "$f"
done
# /home/gokcehan/cmpe230
# /home/gokcehan/Desktop
# /home/gokcehan/Documents
# /home/gokcehan/Downloads
# /home/gokcehan/examples.desktop
# /home/gokcehan/Music
# /home/gokcehan/Pictures
# /home/gokcehan/Public
# /home/gokcehan/Templates
# /home/gokcehan/Videos
# /home/gokcehan/VirtualBox VMs

# show the basename of '/path/to/file/name.txt'
basename /path/to/file/name.txt
# name.txt

# show the basename of '/path/to/file/name.txt' and strip '.txt' extension
basename /path/to/file/name.txt .txt
# name

# generate a sequence to 5
seq 5
# 1
# 2
# 3
# 4
# 5

# generate a sequence from 2 to 5
seq 2 5
# 2
# 3
# 4
# 5

# generate a sequence from 2 to 9 with a step of 3
seq 2 3 9
# 2
# 5
# 8

# you can use 'continue' and 'break' in loops
for i in $(seq 10); do
    if [ $i -eq 3 ]; then
        continue
    elif [ $i -eq 5 ]; then
        break
    fi
    echo $i
done
# 1
# 2
# 4

## case/esac

# Each body is finished with ';;'
# Left '(' is optional
# Multiple options can be given with '|'
# Globs can be used with patterns
# Everything else can be given with '*)'

os=$(uname -s)
case $os in
    ("Linux")
        echo "setting up for Linux"
        ;;
    "Darwin")
        echo "setting up for MacOSX"
        ;;
    "NetBSD"|"FreeBSD")
        echo "setting up for BSD"
        ;;
    *)
        echo "unsupported system"
        ;;
esac
# setting up for Linux

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## FUNCTIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# define a function named 'greet'
greet() {
    echo hello $1
}
# (function is defined)

# define a function named 'greet' (bash only -- not POSIX standard)
function greet {
    echo hello $1
}
# (function is defined)

# statement separator ';' is required for a single line definition
greet() { echo hello $1; }
# (function is defined)

# run 'greet' function with the argument 'world'
greet world
# hello world

# define a function named 'extract' to extract an archive using its extension
extract() {
    case "$1" in
        *.tar.bz|*.tar.bz2|*.tbz|*.tbz2) tar xjf "$1";;
        *.tar.gz|*.tgz) tar xzf "$1";;
        *.tar.xz|*.txz) tar xJf "$1";;
        *.zip) unzip "$1";;
        *.rar) unrar x "$1";;
        *.7z) 7z x "$1";;
    esac
}
# (function is defined)

# run 'extract' function with the argument 'foo.tar.gz'
extract foo.tar.gz
# (archive file is extracted)

```

Related:

* <https://gokcehan.github.io/>

Tags:
    
    #linux #bash #expansions #commands #controlFlow #wildcards
