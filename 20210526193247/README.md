# bboost20: Day 12

Linux Beginner Boost - Day 12 (May 19, 2020)

##  Wednesday, May 19, 2020, 01:25:36PM

### LINUX Terminal

**HINTS**
* tty= terminal device (the current terminal you input commands)
* if you want standard error and standard output use: & e.g. `find / -name '*.md' &>/tmp/find.out`
* `ls /bn /usr/bin | sort | uniq-d | less` this kind of commands can be adjusted to look for the biggest files/programs in your usr/bin as a good use case
* shell scripting is very useful because you can write code and get powerful tools quickly. If you do this in another language like python it would be much more time consuming to add this little for your own purpose needed custom tools.
* `tee` is special because you can pipe into tee and send standard output/standard error to one place and you still get to send it to a pipeline. So you can see what you are doing and monitor your commands.
* `echo &RANDOM` gives back a random number, if you `echo $((RANDOM%8))` you get a number 1-7 and you can loop this: `while true; do echo \$((RANDOM%6 +1)); sleep .5; done` this is not mathematically strict random, its pseudo-random
* escape character: backslash\ not only works in shell it is escape all over the world e.g. pandoc, other programming languages ... also problematic for old school windows because \ there is used for file paths.
* use it if there is a space in it like `ls /tmp/some\ stupid-filename` you can also tab to see what files get listed. if there was a ` stupid-filename` in your dir with space.
* DON'T USE SPACE IN YOUR FILENAME!

#### Brace Expansions

`for i in {a..z}; do echo $i; done` this will give you a standard output of a..z, why care? because you can also count this way. if you change {1..10} easy way: `for i in {01..10}; echo wk$i; done` hard way:
`for i in {1..10}; do printf "%s" $i; done` or `for i in {1..10}; do printf "wk%02d\n" $i; done`
use this version of loop for google (easy way): with strings concat `do dig +short $site.google.com; done` for site in gmail images

#### Command substitutions

* don't use back ticks \` use \$ instead
very powerful because you can nest $ e.g. `echo $( echo $(now) )`

#### Redirection

**HINTS**
* Never `cat` into a pipe.
* `grep pattern file` is standard you can also use `grep pattern < file`
* if you `> filename` you delete the whole content of a file. the file still exists though. If you have no-clobber set you can' do that. But you can use `>| filename` to do it. nice if you run a script under no-clobber-mode and you still need this function.

When do you want to redirect standard error?
case: you wanna do a `find` on your entire OS to look for any file that has set your ID permission. Or to look for all your .md files `find / -name '*.md'` will list you a lot of errors and useless information.
You want: `find / -name '*.md' 2>/dev/null` so you send the stuff to the bit bucket

How to save all the permission denied errors? (it has to be this error)
`find / -name '*.md' | grep 'Permission'` why would that not work: because it would print it to standard error (shell) you would also need to redirect it.
Even with redirect you still get it on your shell though: `find / -name '*.md' | grep 'Permission' > /tmp/foo` that is because standard errors get displayed on shell (default) if you just want it redirected you would need `find / -name '*.md' &| grep 'Permission' > /tmp/foo`

Understanding the pipeline & redirection is very powerful, if you can even add these with some basic shell scripting you can do amazing things: e.g. you combine these \$(find / -name '\*.md' \&| grep 'Permission') this will put the command into a sub-shell and you can loop it `while read -r line; do echo $line; sleep 1, done <<< $(find / -name '*.md' &| grep 'Pe    rmission')`

#### Script

`scrip` every command you enter from now on goes in this "editing script" `cd` and then `cd -` and then `exit` will save your script. This will also contain all of the output and color sequences. So in this form it is not useful yet. However you can play that back the script: or just use the program `haskell`

#### Permissions

good example of some file-types: `ls -dl text-file.txt dir1 bash-script ~/.bashrc /dev/null /usr/bin/sudo /proc/1/cmdline $WEECHAT\_FIFO` all of these are different inodes
* everything is a a file in linux: means really everything is an inode every keyboard, screen, programs that run etc. including /dev/null (bin bucket is not really there it is just a pointer)
* types of 10 digit rwxrwx stuff:
d= directory c= character device l= symbolic link p= pipe (first in first out 5/0, anything you pipe into this file goes into the destiny place (WEECHAT in this e.g.) if you pipe | and redirect > content to a 5/0 (special type of file) it actually sends the data to whatever it is linked to in this cas the WEECAT
![permission](~/Pictures/permissions.PNG)
* proc is a pseudofilesystem: its a read only filesystem and doesnt map directly to memory on a block device. It is showing you information about the processes. `top` and `htop` are looking into /proc when they show you the processes.
* DONT: put a password in an argument to scripts or in your environments (environment variables). Because they are visible to everyone on the system.
* `whoami` shows your username on a server you can also do `who am i`
* in groups people can share files and edit them at the same time.
* /usr/bin/sudo: dangerous file thats why it is red: you need to know about if you do tryhackme - start out as whoever the user is. and then after write/run the program itself is dependent for stopping a person to run. because when anyone runs sudo if they sucessfully run it it gives you root access. because why? because the "s" in the 10 digit permissionline stands for set your id and does have this power to give grant root access to whomever knows the password.  the reason why /sur/bin/sudo has to have set your id is: when that program runs it checks to see if you are who you say you are by checking your password. and if it says so it lets you be whoever you want (root access) over the system. No.1 place to look for escalating permissions.
* to find all the programs that haver set your id "s" run `find / -perm ... no remembered`
* don't use set your id in a script! (it is possible in perl not bash)

----

### Tips

* install `pstree` to see subprocesses or subshellss
* do the (Exercism)[https://exercism.io] bat bash framework to learn in a test driven environment what your shell scripts do.
* create a good linkdin profile. it will definetly help you to get a job in IT. the more specialized you become as an engineer, the more focused you are and linkdin becomes less attractive. But for starters it's great. But you want to improve all your chances so networking in business always pays off.
* open source participation on gitlab/github shows many companies that you can be trusted because they see your code/projects and what not. But it also shows that you can work in a team, and you can make your case, and you don't get triggered by people who disagree with your technical perspective. especially good for software engineers.
* a way to contribute to open source dev. is if you see something that you need fixed. and you fix it an put it on git. A lot of the open source projects need work done, that no one else wants to do. You can even start by helping with the documentation of a project, and ask them if they need help to do their README files. Or filtering their issues on git or answering form requests. This activity will build trust with the community. It also shows that you know how to communicate. Advice: look at a project and try to find out what the issues are. See what they need and if you can offer your service. You need to be known to them so hang out in their IRC, become a part of their community, be on their discord or wherever they are (slack). And over time you get to know where they need help.
* if you commit to a project you need to make sure you don't have too many commits and rebase them.
* every single projects needs testing. If you write good test cases you are needed.
* SKIP advanced keyboard chapter in LINUX book: the how to move your cursor part use vi commands for that (set -o vi)
* faster `clear` add to bashconfig:
clear is a function
clear ()
{
	echo -n $clear
}

### General Notes

* PROJECT IDEA: create a script to detect all your death links on your website like rob has.
* PERL: awesome for using regular expression and finding replace vs. bash
* PYTHON: more organizeed, designed for large codebases that stay around for a long time.
* start practicing on overthewire.org
* PROJECT IDEA: put your ssh-keys into Keypass instead your ~/ this is much more save => ssh authentication, maybe bitwarden works too? Don't keep your private key in ~/ it's not safe.
* PROJECT IDEA: WEECHAT can be used to send msg from terminal to IRC-channel with a script (ipad picture)

### Good practice
