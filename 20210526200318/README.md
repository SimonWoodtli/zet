# bboost20: Day 18

Linux Beginner Boost - Day 18 (May 27, 2020)

##  Wednesday, May 27, 2020

### Topic

***HINTS***
* shell scripts inside your bashrc is a good way if you don't wanna go full script.
`source` takes a file and runs it directly in your shell as if you typed in the contents of the file, while running a script as an executable (like ./greet). If it has #!/bin/bash at the top it runs it in a new bash process. `./script` runs it like a script and any other executable and it creates a subprocess a subshell for that one. So they are different.
* Bashrc means you are sourcing the bashrc and bash_aliases or anything else that gets sourced (`source`) on startup. Your shell is being launched a program, the bash program is being run and then the first thing it does is sources/imports those initial files.
* you need very little vim-script knowledge, because you can create all your filters, extensions and gismos in bash and they will work everywhere in vim with magic wands.
* if you want your colors in your script without a library. You can also add the colors individually to every script.


Greet5:
in order to run a function you need to call it. greet () { content }= variable now on a new line `greet` in your script means it will call it. If you add Text before and after that you get that too (see greet5)

Greet7:
This script allows you to enter your name in the prompt and get a response back. `$reset` at end of line is to stop color bleed

Greet8:
Same as 7 but you don't need to echo. `read -p` allows you to put all in one line. If you use `declare -r` in a script it becomes a constant (read only).

Recursive Loops: true loops

Nyan1:
Idea how to repeat a script or parts of it. This will run endless! C-c to exit
Nyan2: sleep to make it run slower, number stands for seconds
Nyan3: all on one line and clear screen first
Nyan4: add declare
Nyan5: add color grey and text nyan gets grey and Cat is white
color.bash: add your colors to your new color library. If you use sufix you will know this file is just for storage. only give imported or sourced files a .bash extension. That way you keep things organized (not mandatory but useful). This color.bash library is not exported, that is not a problem as long as it is being imported. This library will work for the current directory and all sub dirs of it.

Nyan5.1: remove the color declares as they are now stored in your library. But you must import them (in bash called sourced).
color.bash: now since nyan and greet both have a dependency on your color library that is usually bad. Usually if you have your final version you want to cut/paste those lines out. So you can get rid of this unnecessary dependency. This is only true for the bash scripting world!
* COLOR FIX for scripts: use `cpick` from robs student on github and vims magic wands to paste them into your scripts.

rndcolor: script that has a function for random colors. you use the modul operator which gives you the remainder of a division. `echo $((9%3))` would be 0 because there is no remainder.
`echo -en` the -e helps it to escape it. However if you mange not to use it it is better code.

Nyan6: echo line needs to change {grey} to (...) because it is now a command substitution.

rndcolor: maybe you don't want it to be a script and you want it to be really efficient, and available to your entire environment. You can do that by exporting an envrionment variable function in your bashrc.
You can put it anywhere in your listed paths. To see a list `path` in the shell will show you. I put it into Repos.../scripts. Now you can use this script without ./ :). Because it is in a "safeplace" where the shell does not require to type this anymore.

Nyan7: runs withou ./ because we have rndcolor integrated in our environment paths.

waffles1: First script with if condition.
* when in doubt quote it line 5 (if "\$answer"). Otherwise if you don't quote it and you run it but don't enter anything into the shell it just quits. So if you want to know if someone entered a blank answer you can't. Like if you make an if answer blank line...
* single equal sign invoke another type so we need \== because it is bash.

waffles2:
give answer back: Okay then with rndcolor.

waffles3:
make Q&A randomcolor

waffles4:
answer yes to both question possible now. also more colors

waffles5:
now this script allows all kinds of answers starting with "y". like yeah yippi or whatever. Including uppercase. name of it: gloves/wildcards. You could also use a regular expression for it.

waffles6:
use regular expression to allow more then just "yes" as an answer. actually it allows any answer now. But we only want positve/confimative answers to allow the second Q.

waffles7:
* if you use regular expressions slowly build them up and test things. Instead of trying to find a solution that has many tiny stones.
use a carat so that the beginning of needs to be y. ^y. but it still does not react to upper/lower case.

waffles8:
range will allow upper/lower case answers with Y,y as positive answers. But also yellow as it begins with y too.

waffles9:
Now we only want Y,y but also it needs "es" at the end.

waffls10:
force answer to be uppercase with ^^. If you want to force lower case ,,. if you wanna force it to be initial case ^. THis will allow all kinds of upper/lower case Y answers.

waffles11:
the dollar sign makes only yes/Yes possible. to allow a bit more answer starting with ye we can make it optional by grouping it.

waffles12
group ye with (). and the ? means there can be 0 or one of those. now Y and yes work as answer. But ye won't work.

waffles13
the pipe lets more answer be positive. like yeah and yeppers. but yep won't work.

waffles14
what do they all have in common? they all have a "e". So we can make another group. so now it can be yep or yeppers p(pers)? and its optional.


----

### General Notes

* return values are limited to integrals. So it is nice to create a script that prints out return values. because most of the times you want to print stuff out.
* in your bashrc the exported Paths all have a \ at the end. This is for line continuation otherwise all paths would be one one line.
* REGEX= regular expression

#### Common Practice

1. bb

#### Tips

1. if you do `chmod` and use octal be aware of the u mask. If you don't use the default u mask and you do chmo 700 it will screw it up. In that sense symbolic is a bit safer especially with recursive file changes. DONT: use recursive on a directory and all the sub files/dirs! Octal has different meaning on files and dirs. symbolic notation is OK for that.
1. Trick: anytime you have a file that you want to be imported and not actually executed you can give it a suffix. In LINUX&UNIX it is very unusual for files to have sufixes unless they are data files.
1. `path` will show you all the places Linux is looking for to run things like scripts, programs etc.
1. vic in your bashrc allows you to open any scripts anywhere you are `vic $(scriptname)` or `vi $(s.name)`.
