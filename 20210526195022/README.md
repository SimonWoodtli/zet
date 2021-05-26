# bboost20: Day 16

Linux Beginner Boost - Day 16 (May 25, 2020)

##  Wednesday, May 25, 2020, 01:27:53PM

### TMUX

***HINTS***
* you can script everything with tmux, very powerful
* window managers don't matter too much. Don't hotkey everything on it. You just become dependent on it. It is better to go the terminal configured tmux route.
* tmux, vi, bash shell combination. and think of that as your desktop manager or your IDE. = very powerful. magic wands from vi (integrate shell commands).
* tmux allows to copy/paste between windows
pane and window are not the same. pane= a split of a window. window= is another separate window (screen) that can be further divided into multiple panes.
* TMUX stays on. Even if  you leave the server. even if power goes out and you have to reconnect.
* TMUX allows to pare programming. you share the keyboard. one person can have control and type and give it to the other vice versa. (check out wemux, for this)
* you can nest sessions. one tmux session within another session
* screen is installed on all the computers since the 70s and is still on UNIX. it is the "older brother" of tmux. If you end up on a system that has no TMUX and you still want to be productive, it makes sense to change the tmux default metakey and hotkeys to the ones from "screen".
* tmux added a few features: split panes, script sessions, full color support and less bugs
* window manager are not really useful. combination tmux-vim-bash is much more powerful. also don't get too dependent on hotkeys from the window manager as that may easily change (keyboard shortcuts)
* think of bashshell,vi,tmux combo as your desktop manager, you can use vi magic wands (integrate shell commands), also add lynx and mutt to it
*tmux windows: seperate screen tmux pane: you can have multiple on same window to have mutlipe terminals on same window
* why tmux: screen splitting, it is on all the time (if you are on a remote sytem tmux stays connected, even when you disconnect from the server), even if your server crashes you can start it up and get your tmux session back. multiple people can connect into tmux (check out wemux)
* tmux allows para programming: share same keyboard one has control the other is watching and can take control if he wants.
* ressurcet let you save your pane size and session when you reboot in tmux. rob is not a fan of it.
* you should hack your tmux so you don't have any screenborders. (install dir robs gitlab)
* if you want to save tmux sessions and have the pane exactly the same after reboot you can use the "resurrect" extension

TMUX Commands:
\# the way tmux works is you hit first a metakey and then you hit a hotkey. default is CTRL-B. We changed that to CTRL-A but you could change that to anything you like.
`CTRL-A \ or |` split window vertically
`CTRL-A - or _` split window horizontally
`CTRL-A c` new window
`CTRL-A 1-9` switch between windows
`Ctrl-A a` last window
`C-a w` shows you all of your windows
`C-a r` reload config: if you mess with your config a lot
`C-a d` dettach tmux, now you can ssh into this computer from another device and `tmux -a` from there and it will load your last session. (also works for servers of course)
`C-a x` kill a pane if it is stuck
`C-a C-x` kill all sessions (all windows,panes in them)
how to kill only one session?


TMUX copy-mode into VI:
`C-a [` select: space navigate: hjkl confirm: enter
`C-a ]` in another tmux window in vi while insert-mode is active: paste

TMUX vimlike status keys:
\# This mode also has history, navigate with arrow keys
`C-a :` now you can enter your set commands
e.g. :swap-pane -s1 -t2 will swap your panes

Resize Panes:
`C-a` and hold CTRL now spam hhh,jjj,k,l to resize the active pane

checkout and tweak:
`set -g repeat-time 200` because your keystrokes may be different from your OS settings

Turn on clock: `C-a t`


### Bashscripting

***HINTS***
* Octal to make files/dir executable or change permission in general has different codes for files and dir. chmod 700 on a file is not the sam as chmod 700 on a dir.
* why does an empty executable file still run (./file), no error? I will run this script using whatever the default shell is. (default shell is bash).
* bash is an interpreted language not compiled
* the golden robot in star wars is an interpreter: his job is to translate from one language to another. That is also what interpreters do in computer languages.
* you cannot run anything on a computer that is not compiled code.
* an interpreter takes your code and converts it into system calls and sends those system calls to the kernel and runs it for you. the only thing that runs is bash
* the golden robot in star wars is an interpreter: his job is to translate from one language to another. That is also what interpreters do in computer languages.
* you cannot run anything on a computer that is not compiled code.
* an interpreter takes your code and converts it into system calls an sends those system calls to the kernel and runs it for you. the only thing that runs is bash.
* the only thing that can run on a linux/unix system is bash (called compiled ELF binary). They get executed in a true sense, the actual kernel runs it.
*java and python are compiled and interpreted languages. They are compiled to byte-code which is pseudo binary thing that is closer to machine code but it is not. Byte-code can be passed and dealt with much much faster.
* go is compiled and not interpreted
* ruby, javascript, bash are interpreted
* you have languages that are compiled like c that run as elf binaries. They are as close to the hardware as they can be. (c, c++, rust, go)
* julia, crystal, rust are modern lanuguages that use the l-vm to help them get to an elf binary
* kotline is java compiles it to byte-code and the byte-code then runs.
* java, csharp and python are unique they actually make byte-code.
* in javascript the compilling happens for you. it happens on the first time you run it. you get a pyc file in your directory.
* the way to categorize languages is to ask how close they are to the "iron", how close to machine code are they, how close to assembly code
* c was written to develop the UNIX operating system, so it is the closest to the hardware. They did that because the first version of UNIX was written in assembly and they wanted to do something that was a little faster. So they invented C to redo UNIX. That is why you will never see C go away. It is fundamentally tied to UNIX&Linux and everything that came from that.
* bash is an interpreted, non compiled language that requires the bash interpreted to be on the system.
* POSIX, bash and dash are shells. POSIX is not meant to be an interactive shell, it is to be super fast.
* The debian team threw out the bash shell for startup because it got too bloated (with interactive user shell stuff) and used dash instead. It saved startup time by about 15min!
* bash has POSIX compliance but it has extensions that aren't. You can write POSIX compliant scripts. But in that case you would rather want bin/sh
* the reasons why a `echo hello` bash script is faster then an executable go equivalent is because of sub-shell. When you run a script that is in the same language as your running shell it always beats a compiled executable. No matter how minimal including C code. Rob thinks it has something to do with the fork process to run the sub shell.
* when you run a script using `source scriptnamesh` it is different it is not creating a sub process and run it there. It literally copying every line and running it in your current shell. When you are using a shell script you can cause it to run right now it that shell environment. And that saves a lot of time.
* first line is the shabang line (#!...) which is needed to give the run a hint which language it is or which interpreter to use. The OS reads this first two characters to see oh, with what do you wanna run this? /bin/bash /bin/python3 or what not. And then it will send the rest of the file as standard input to this bash, python3 or what.
* env (environment) knows where the language on your OS is installed and can then run it `env bash` will run you bash input `env python3` python `env perl` perl stuff. However using the shabang line and the path to the language is more secure like \#!usr/bin/python3. Making your script portable can be achieved with go or other ways. You don't have to make your script become more insecure by using the environment. Also the env will first look for the program and then send this script to the specific language which is another step and slower.

make file executable:
`chmod +x` or `chmod u+x` to be more precise you can also do `chmod u+rwx` or `chmod u=rwx` `chmod u=rwx,go=` They all give different permissions


## When to Use What?

* **Aliases** when never run from anything but an interactive shell.
* **Exported function** when not using anywhere but Bash.
* **POSIX script** when want to share the most or run from *anything*.
* **Bash script** when more than POSIX is required but not enough to compile.
* **Node script** when JSON manipulation or web-centric operations required.
* **Python script** when automation, ML, or math-intensive operations required
* **Go program** when compilation is required.
* **Rust program** when compilation without garbage collection is required.
* **C program** when finely tuned, low-level compilation is required.
* **Assembly code** when ultra low-level compilation is required.

### Lesson 3

* modern bash is basically as powerful as perl
* checkout "declare" in `man bash` you can do a lot more than just
strings in bash. However the default in bash is strings but this can be
changed. local variable declaration
* why ./greet? because your computer does not know where greet is. and
for your safety it does not assume it is in your current directory. if
you want to put your greet script into a safe place you would need to put
it in your `path`.
*.local is the place where you should put all your bin stuff./home/xnasero/.local/bin rather than xnasero/bin.
*read the bash manual lots of stuff that is only in there!
* semicolon and line return are the same in bash.
* if you wann make a loop just add `done` at last line

echo "hello" in bash is a statement (in a script)
if you want "echo hell" to become a variable:
name=World
echo "Hello World" (you would probably want to use your variable: `echo
"Hello \$name`

use `watch ./greet` to get your script running every 2 seconds
`while true; do ./greet; sleep 1; clear; done`
now you can run this constantly updated running script in a tmux pane and
edit your script in another pane. This is super interactive! its like
a debugger

instead of `name=World` use `declare -i name=Simon` in the greet script. this will force the interpretation of the string
Simon into an integer (which is a number) that is why it will output 0.
However if you change the name=Simon to name=3. it will work and O:3.
However this is only necessary if you wanna do calculations with your
script. If you just want the 3 printed no declare is needed just
`name=3`.

Parameter Expansion
```
#!/bin/bash

name=Rob
echo "Hello ${name,,}" (,, will print all in lowercase, ^^ all
upperc.)
```

----1
colorful pane debug
#!/bin/bash

name=Rob
echo -n "Hello $(name)"

now in shell: `while true; do ./greet | lolcat; sleep .25; done
----2
color in script
#!/bin/bash

color="\033[38;2;19;81;143m"
name=Rob
echo -en "{color}Hello $(name)"

or: (no escape sequence needed in echo)
#!/bin/bash

color=$'\033[38;2;19;81;143m'
name=Rob
echo -n "{color}Hello $(name)"

or: with resetting colors
#!/bin/bash

color=\$'\033[38;2;19;81;143m'
reset=$'\033[0m'
name=Rob
echo -n "{color}Hello ${reset}$(name)"

same script but instead comment echo and use printf
printf "%sHello %s\n" $color $reset $name

----

### General Notes

* bbb

#### Common Practice

1. bb

#### Tips

1. bb
