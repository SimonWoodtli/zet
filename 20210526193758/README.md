# bboost20: Day 13

Linux Beginner Boost - Day 13 (May 20, 2020)

##  Wednesday, May 20, 2020, 01:26:09PM

### Processes

***HINTS***
* `pstree` is good to see how everything is managed
* if your system runs out of processes it can't function any longer, it will freeze.
* daemon is a servant, it is a thing that serves you and helps you out. Comes from BSD, because Berkeley's mascot is a devil. It is another word for server or server process= runs continuously and answers you. * sometimes you see a d at the end of a process and it is a directory (confusing). But that is usually .d and not just d.
* X(xorg) now: lightdm is the one that provides the graphics in the windows. and then you have a desktop and windows manager.
* if you kill a parent process the child will die too unless you have no hubs set on it.
* if you kill a process and it does not die it can (rarely) become a zombieprocess. That is not owned by anyone. hard to clean them up
* a webserver is technically a daemon
* background processes are these days not very useful. Because terminal multiplexer. However when you are running scripts background processes can make your bash faster. as fast as languages that have high concurrency like go or javascript.
* subprocess is a child of a parent process.

**Check if a program is not working:** `ps -ef | grep Discord` a modern way to do that `pgrep -il discord` so you can see which process to kill if you think one is lagging.
`pkill name` will kill the process with the name you want. `pkill Discord` kills parent process and all children too. It will do this nicely to your system.

**common mistak** backgrounding a running process.  e.g. `vi file` and exit with CTRL-C because you don't know how to get out of vim. This process will go in the background now as stopped. To see all your processes like this: `jobs` to bring those stopped processes back `fg` and now you can cancel those processes properly:
```
CTRL-S= suspend a process to put it back CTRL-Q
CTRL-C= sends the current process an interrupt signal
CTRL-D= sends an end of file (character).
```
`ps -ef`

### ENVIRONMENT

***HINTS***
* use shell functions instead of aliases. Keep them at minimum.
* your shell is a running program. When you are opening a shell and write every single line gets executed. So it is like a "live" running program.


What is the environment? It is everything that is happening in your shell environment. That includes variables, settings (like set -o vi), functions.

`$(which top)` this will tell: run this command "which top" as  an expansion.
export:
**when does it make sense to save a bash function vs. a script vs. an alias?**

#### BASIC BASH PROGRAMMING

***HINTS***
* POSIX scripts are way faster than bash scripts. So if you don't need anything else than basic POSIX scripting stick with that. However bash provides things that POSIX does not (regular expression matching etc.).
* if you need JSON manipulation or something quick with javascript you would use Node.
* if you need something for machine learning or math intensive operations you would use Python.
* if you use compilation of any kind and want to be able to share it across platforms you would use Go.
* if you need the same thing without garbage collections you would use Rust.
* if it is anything else than those you would use C. Or Assembly if needed.
* difference between prinft and echp? All coding languages have this logic: they specify the formation differently. called formation specifier. `week=5` `echo wk$week` O: wk5 `echo wk0$week` O: wk05 what if you wanna change week? with echo you have to change the variable `week=14` `echo wk0$week` O: wk014 maybe you don't want 0? printf works differently: `printf "wk%02d" $week` O: appends wk14 on at beginning of your prompt. If you change the weeek to `week=5` now your `printf "wk%02d" $week` O: appends 05xnasero@PI4 at your prompt so automatically dealed with the 0. If you want a regular output you need "wk%02\n" so it is a more precise print than echo.
* zsh zhsell won't let you export functions. with bash you can do that.
* normally functions go inside a program of some kind.
* . is the same as source. but . is also POSIX compliant. when you use . on a file it says to every single line of this as if you were typing it like "command by command" CONFUSION: if you do ./ it is the "present directory" if you do `. ` it means run the current directory
* if you use `source functionname` you do not need #!/bin/sh at the beginning of your script because it will run it with the current shell (only works for bash).
* you cannot use `export -f` in zsh export -f stands for export function.
* `exec bash` to reload/refresh your .bashrc
* in vi all the environment is stored in the current running bash interpreter. that is your parent shell. That is why you have to refresh your shell if you want the changes accepted.
* to unexport a function `unset functionname` `unset greet`
* in vi you can add a function to your bashrc to add it permanently. Vi wildcards are a convenient way to do this: `.!type greet` would print it right into your vi. alternatively you can also do `type greet >> ~/.bash_aliases`
* if you add a function to your bashrc instead of `export functionname` at the end you can do `} && export -f fuctionname`.(} is end of body) This will test if the function works and if it does it will export it.
* `kill -9` is very bad practice unless you have to. It will kick a process out of memory and won't give it any time to prepare/recover.
* `printenv` will print out your whole environment
* install your own shell in an enterprise company like a bank will get you fired, however you can load your dotfiles with all your configurations. maybe aliases not.

environment variable= how you store some bit of information about this

start learning writing variables:
`name=xnasero`
`echo $name` will give "xnasero" back.
`echo Hello World` it displays Hello World. In other words it runs.
`echo "Hello\n $name"` now we used this variable
`printf "Hello %s\n" $name`
\# These are all builtin functions that are a part of bash.

How to write a function? \# no one ever uses a function in a live shell session. this is just an example that it can be done

`greet () { TODO: HIT ENTER
\> tis will pop up. it tells you hey I am ready to take some input for this function. It's called the body part of a function.
`echo "Hello there" HIT ENTER
} HIT ENTER

Now: if you `type greet` it will tell you that it is a function. As soon as you write a function it is the every bit the same as any other command on your system.

If we chante the variable to `greet () { echo "Hello $1"; }` we can add text after function that gets added (thanks to the special variable \$1).
`greet Simon` O: Hello Simon

if you are creating a bash script with this function we just created. And want to run this function it wont work. WHY? Because the greet function has not been exported and is therefor not available for child processes. So what does export do? It takes a variable in the case of bash or a function and makes it available to all child processes. Primary reason for this is security. So Functions can only run if they are exported. So `export -f greet` will now make a script run.

----

### General Notes

* d in systemd stands for daemon, init still exists and rules over all, it is a central program that runs everything else (master control program). It is process ID 1 and is always process ID 1

#### Common Practice

1. bb

#### Tips

1. `cmatrix -c default` runs matrix transparent!
1. to test graphics: `xeyes` or `xlogo` better though: `glxgears`
1. `ls -1 | wc` shows you how many file/dirs you got
1. `hyperfine` to see how long a program needs to do its thing.
1. if you write a build script in bash. you don't need to list wait for pid. you can simply do "wait"
