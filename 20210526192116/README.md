# bboost20: Day 11

Linux Beginner Boost - Day 11 (May 18, 2020)

##  Wednesday, May 18, 2020, 01:24:56PM

### Shell Basic commands

***HINTS***
* if an option in a `$ command`  has [] it means it is a optional option e.g. [-H] if you are reading the manual so you don't have to use this option unless you want.
* use `tldr command` instead of `man command` to get a short overview of main commands instead of huge manuals.
* how can I use `ddgr` together with lynx instead of getting pulled into GUI browser? `ddgr searchterm` uses duckduckgo and gives you top10 keysites (very clean). If lynx stays as slow as it is this might be a better option.
* read man man first: `man man` to get to know how to read the manual
* use `view file` instead of `less file` it is vim in reading mode

** hard link vs soft link**

A hard link is an additional pointer that points to the exact same location of memory on your storage device. A symbolic link is not a pointer, it's its own thing.
A symbolic link is an inode. A hard link is a pointer to an inode. An additional hard link is another pointer to an inode.

If you create a soft link to a file and delete the file. The soft link is "broken" because it does not know what to execute anymore (windows shortcut style). However if you have 3 hard links to a file you can delete the original file and the other 2 hard links will still work.

If you `ls -l` a dir you get a 10digit -rwx--r-- 1 sero sero .... the 1 says you have one hardlink to the file. If the number changes that means you have more hard links on your system to this file.

Hard links are way faster then symbolic links. Anytime you do something with a soft link the file system has to redirect.

You can have symbolic links that point to hard links. "All soft link are linked to hard links, true?"

* When you make another hard link you are not making a copy of the file. You make another low level reference to the same file.

* Hardlink is a reference to memory.
* always put \$PWD in front of a symbolic link. So when you move the file around it resolves properly and still knows where it is and where it points to.
* you can cause hard links to not follow symbolic links. Hard links can get you into recursive loop, and many other things, they are highly efficient.
* symbolic links can be filtered out of finding commands and follow commands and stuff like that. They are regarded as not real files. And they can be distinguished from hard links. Hard links can not be distinguished from the other file.

When to use hard links when to use soft links?
hard links are really good for recreating file structures that need low level access to the system. That are never going to be involved in any kind of recursive function.

Symbolic links are really good to use in your home directory. To go and say hey go over there use that. You can use e.g. for your config files in ~ to link to your Repo dotfiles. You can use symbolic links to point to different mounted drives (you can't do that with hard links).


#### Wildcards/gloves

* Regex and Wildcards are similar but not the same. "?" in regular expression means one or more of the same thing. A "?" in a glove means one of anything.

* glovestar is when you can use the `ls **README.me` to see all the readme in the pwd and in all the subdirectories or your pwd. This needs to be enabled in your shell. As normally ls would only look for files in pwd. add this to bashconfig `shopt -s globstar`
* by default the star (*) does not list hidden files. If you add `shopt -s dotglob` they get listed too
* there are more settings for wildcards that change the defaults rob uses the following in his bashrc:
set bell-style none          # MAKE IT STOP!!
#set -o noclobber            # paranoid? use >| for everything
shopt -s checkwinsize
shopt -s expand_aliases
shopt -s nullglob
shopt -s globstar
shopt -s dotglob
shopt -s extglob
shopt -s histappend
HISTCONTROL=ignoreboth
HISTSIZE=5000
HISTFILESIZE=10000

Rob recommends to set noclobber (uncomment it) for beginners. It stops you from destroying your files by doing something you were not thinking was dangerous. I think even better is setting an alias to mv -n which sets noclobber just for move.

* check out the `find` command he is also important

#### Character Ranges

tr is important for ranges. it translates from one character to another.
Ranges are a way to say "I want all the letters from A-Z for example.

**`mv` warning**
if you move a file/dir into another place where this name for the file/dir already exists

#### Redirection

* to overwrite a files content use `echo hello world >| hello to you` this will ignore the normal error msg you get.

----

### General Notes

* if you `vi /bin/ls`  yout can't read the content because it is binary and not text.
* ALIASES: don't overuse them you don't wanna become dependent on them. If you are on a system where you don't have your aliases you may make mistakes. So for the whol `mv -n` noclobber thing a better advice is to: if you think you are moving a file to a directory always add a / at the end. e.g
file: foo dir: fooo now use `mv foo fooo/` if it is not a directory you will get an error and you can't do it!
* if you delete a file on accident and its a GIT repo you can get it easily back: `git checkout filename` will restore the last commit.

#### Common Practice

1. bb

#### Tips

1. You can `vi .` to vim into your pwd and edit/rename/delete files and directories. (exit with :q!) so you don't save it if you did delete something on accident.
1. configure vic as a function contains: it allows you to open an executable file in your system no matter where it is (super useful!)
Add to bashconfig:

vic is a function
vic ()
{
	vim $(which $1)
}

e.g. `vic pdf` `pdf themagicbook.pdf` will open this book wherever it is

Trash script: sets up a trash directory, so instead like with rm you still have your things and might decide to keep them. (less likely to screw up).

trash is a function
trash ()
{
	[[ -z "$TRASH" ]] && return 1;
	mkdir -p "$TRASH";
	mv "$*" "$TRASH"
}

1. So don't ever do any kind of work if it is not a GIT repo.
1. IF you put `rm -rf` into a script that does not check the argument you can end up destroying your system. So always check the input parameter.
1. if you `python3` you can code directly python in the shell.
1. putting commands into the shell is actually the same as coding. If you put many of those together in a script that's a code. And each line is also like a code. with option arguments if statements etc.
1. you can change your interactive shell on linux from bash to python, ruby, node, perl ... amazing but weird and annoying :)
searching with a terminal browser is more effective than the manual reading, usually. So you should really start using it especially for searching command based information. Like how to do stuff on the shell.
1. read `man bash`
