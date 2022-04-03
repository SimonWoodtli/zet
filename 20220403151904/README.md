# Basic unix/linux commands 

```sh
#!/bin/sh

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## POSIX CONVENTIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ls (1) - list directory contents

# Common options for `ls`
# ~~~
# `-a` to not ignore entries starting with .
# `-l` to use a long listing format
# `-h` to print human readable sizes
# `-t` to sort by modification time, newest first
# `-S` to sort by file size, largest first
# `-r` to reverse order while sorting
# `-R` to list subdirectories recursively

# POSIX Utility Argument Syntax
# ~~~
# utility_name [-a] [-b] [-c option_argument]
#     [-d|-e] [-f [option_argument]] [operand...]

# POSIX Utility Syntax Guidelines
# ~~~
# Guideline 4:
#     All options should be preceded by the '-' delimiter character.
#
# Guideline 5:
#     One or more options without option-arguments, followed by at most one
#     option that takes an option-argument, should be accepted when grouped
#     behind one '-' delimiter.
#
# Guideline 10:
#     The first -- argument that is not an option-argument should be accepted
#     as a delimiter indicating the end of options. Any following arguments
#     should be treated as operands, even if they begin with the '-' character.

# Exercise
# ~~~
# Read the rest of the utility conventions at:
# https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## MANUALS AND DOCUMENTATION
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# whatis (1) - display one-line manual page descriptions
# apropos (1) - search the manual page names and descriptions
# man (1) - an interface to the on-line reference manuals
# manpath (1) - determine search path for manual pages
# info (1) - read Info documents
# help (information about builtin commands) (shell builtin)
# type (information about command type) (shell builtin)

# display one-line manual page description of 'ls' command
whatis ls
# ls (1)               - list directory contents

# display descriptions of all available commands matching wildcard pattern 'ls*'
whatis -w ls*
# ls (1)               - list directory contents
# lsattr (1)           - list file attributes on a Linux second extended file system
# ..
# lstat64 (2)          - get file status
# lsusb (8)            - list USB devices

# display all available commands with descriptions containing word 'directory'
apropos directory
# alphasort (3)        - scan a directory for matching entries
# basename (1)         - strip directory and suffix from filenames
# ..
# vdir (1)             - list directory contents
# versionsort (3)      - scan a directory for matching entries

# display all available commands with descriptions containing words 'list' and 'directory'
apropos -a list directory
# chacl (1)            - change the access control list of a file or directory
# dir (1)              - list directory contents
# File::Listing (3pm)  - parse directory listing
# ls (1)               - list directory contents
# ntfsls (8)           - list directory contents on an NTFS filesystem
# vdir (1)             - list directory contents

# display manual page of 'ls' command
man ls
# (displays manual page in a pager -- press 'q' to quit)

# display one-line manual page descriptions of 'printf' command and library call
whatis printf
# printf (1)           - format and print data
# printf (3)           - formatted output conversion

# Man Sections (see `man man`)
# 1. Executable programs or shell commands
# 2. System calls (functions provided by the kernel)
# 3. Library calls (functions within program libraries)
# 4. Special files (usually found in /dev)
# 5. File formats and conventions eg /etc/passwd
# 6. Games
# 7. Miscellaneous (including macro packages and conventions)
# 8. System administration commands (usually only for root)
# 9. Kernel routines [Non standard]

# display manual page of 'printf' command
man 1 printf
# (displays manual page in a pager -- press 'q' to quit)

# display manual page of 'printf' library call
man 3 printf
# (displays manual page in a pager -- press 'q' to quit)

# display search path for manual pages
manpath
# /usr/local/man:/usr/local/share/man:/usr/share/man

# display info page of 'ls' command
info ls
# (displays info page in info viewer)

# display manual page of 'cd' command
man cd
# No manual entry for cd

# display shell help of 'cd' command
help cd
# cd: cd [-L|[-P [-e]] [-@]] [dir]
#     Change the shell working directory.
# ..
#     Exit Status:
#     Returns 0 if the directory is changed, and if $PWD is set successfully when
#     -P is used; non-zero otherwise.

# display command type of 'ls' command
type ls
# ls is /bin/ls

# display command type of 'cd' command
type cd
# cd is a shell builtin

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## PATH NAVIGATION
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Unix paths
# ~~~
# Root directory is denoted with `/`
# Home directory is denoted with `~` (expanded by shell)
# Path separator is `/`
# Current directory is denoted with `.`
# Parent directory is denoted with `..`
# Absolute paths are those that start with `/`
# Relative paths are those that does not start with `/`

# pwd (1) - print name of current/working directory
# cd (change directory) (shell builtin)

# display present working directory
pwd
# /home/gokcehan/cmpe230/basics

# change working directory to parent
cd ..
# (changes to parent directory)

# change working directory to 'basics' from current directory (relative path)
cd basics
# (changes to 'basics' directory in current directory)

# change working directory to '/etc' (full path / absolute path)
cd /etc
# (changes to '/etc' directory)

# change working directory to home directory of current user
cd ~
# (changes to home directory)

# change working directory to home directory of current user
cd
# (changes to home directory)

# change working directory to previous
cd -
# (changes to previous directory)

# Exercise
# ~~~
# Examine the file system (see `man hier`).

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## FILE/DIRECTORY OPERATIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# mkdir (1) - make directories
# rmdir (1) - remove empty directories
# touch (1) - change file timestamps
# rm (1) - remove files or directories
# cp (1) - copy files and directories
# mv (1) - move (rename) files
# ln (1) - make links between files

# create and then remove 'demo' directory (empty)
mkdir demo
rmdir demo
# (creates and then removes 'demo' directory)

# create and then remove 'demo' directory (non-empty)
mkdir demo
touch demo/foo.txt
rmdir demo
# rmdir: failed to remove 'demo': Directory not empty

# recursively remove 'demo' directory along with its children
rm -r demo
# (removes the directory)

# create a nested directory 'demo/foo/bar/baz' along with its parents
mkdir demo/foo/bar/baz
# mkdir: cannot create directory ‘demo/foo/bar/baz’: No such file or directory

# create a nested directory 'demo/foo/bar/baz' and then remove them
mkdir -p demo/foo/bar/baz
rmdir /demo/foo/bar/baz
rmdir /demo/foo/bar
rmdir /demo/foo
# (creates 'demo/foo/bar/baz' along with its parents and then removes them)

# copy 'demo/foo.txt' file to 'demo/bar.txt'
mkdir demo
touch demo/foo.txt
cp demo/foo.txt demo/bar.txt
# (copies the file)

# copy 'demo' directory to 'demo2'
cp demo demo2
# cp: omitting directory 'demo'

# copy 'demo' directory to 'demo2'
cp -r demo demo2
# (copies the directory)

# move 'demo2' directory to 'demo3'
mv demo2 demo3
# (moves the directory)

# remove 'demo3' directory
rm -r demo3
# (removes the directory)

# Hard Links vs Soft (Symbolic) Links
# ~~~
# A hard link is a new entry pointing to the same file content in the filesystem
# A soft link is a new link file pointing to a file path
# Hard links continue to work when the original file is moved or removed
# Soft links only work when there is an existing file in the linked path
# Hard links can only be used for files
# Soft links can be used for files or directories
# Hard links can only be used for a file in the current filesystem
# Soft links can span over different filesystems

# create a hard link named 'hard' to 'demo/foo.txt'
ln demo/foo.txt hard
# (creates the hard link)

# remove 'hard' file
rm hard
# (removes the hard link but not the file)

# create a soft link named 'soft' to 'demo/foo.txt'
ln -s demo/foo.txt soft
# (creates the soft link)

# remove 'soft' link
rm soft
# (removes the soft link but not the file)

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## FILE DISPLAYING/EDITING
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# file (1) - determine file type
# cat (1) - concatenate files and print on the standard output
# head (1) - output the first part of files
# tail (1) - output the last part of files
# more (1) - file perusal filter for crt viewing
# less (1) - opposite of more
# gedit (1) - text editor for the GNOME Desktop
# nano (1) - Nano's ANOther editor, an enhanced free Pico clone
# vi (1) - Vi IMproved, a programmers text editor

# create 'hello.c' file with content up to 'EOF' (heredoc literal)
cat << 'EOF' > hello.c
#include <stdio.h>

int main(void)
{
	puts("Hello World");
	return 0;
}
EOF
# (creates the file)

# print 'hello.c' file to the standard output
cat hello.c
# #include <stdio.h>
#
# int main(void)
# {
# 	puts("Hello World");
# 	return 0;
# }

# determine the type of 'hello.c' file (traditional human readable output)
file hello.c
# hello.c: C source, ASCII text

# determine the type of 'hello.c' file (mime type output)
file -i hello.c
# hello.c: text/x-c; charset=us-ascii

# compile 'hello.c' source into 'hello' binary
gcc hello.c -o hello
# (compiles the file)

# run 'hello' binary
./hello
# Hello World

# determine the type of 'hello' file (traditional human readable output)
file hello
# hello: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=2500b0e3243b8052a5cdb06925e15447ea54c4ba, not stripped

# determine the type of 'hello' file (mime type output)
file -i hello
# hello: application/x-sharedlib; charset=binary

# display the first 3 lines of 'hello.c' file
head -n 3 hello.c
# #include <stdio.h>
#
# int main(void)

# display the last 3 lines of 'hello.c' file
tail -n 3 hello.c
# 	puts("Hello World");
# 	return 0;
# }

# display the last lines of '/var/log/syslog/ file and then continuously follow the file and display new output
tail -f /var/log/syslog
# (displays the file -- interrupt with Ctrl-c to quit)

# display 'hello.c' file with 'more' pager (one page at a time)
more hello.c
# (displays file one page at a time -- press 'space' for next page if any and 'q' to quit)

# display 'hello.c' file with 'less' pager
less hello.c
# (displays file as a pager -- press 'q' to quit)

# open 'hello.c' file with 'gedit' editor (editor by GNOME)
gedit hello.c
# (opens the file)

# open 'hello.c' file with 'nano' editor (editor by GNU)
nano hello.c
# (opens the file -- press 'C-x' to quit)

# open 'hello.c' file with 'vi' editor (POSIX standard editor)
vi hello.c
# (opens the file -- type ':q' and press enter to quit)

# open 'hello.c' file with 'vim' editor (vi improved)
vim hello.c
# (opens the file -- type ':q' and press enter to quit)

# start tutorial for vim
vimtutor
# (opens the tutorial)

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## DISK OPERATIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# df (1) - report file system disk space usage
# du (1) - estimate file space usage
# dd (1) - convert and copy a file

# show file system disk space usage
df
# Filesystem     1K-blocks      Used Available Use% Mounted on
# udev             1994708         0   1994708   0% /dev
# tmpfs             403720      1556    402164   1% /run
# /dev/sda1       10253588   5952044   3760976  62% /
# tmpfs            2018584    100208   1918376   5% /dev/shm
# tmpfs               5120         4      5116   1% /run/lock
# tmpfs            2018584         0   2018584   0% /sys/fs/cgroup
# tmpfs             403716        28    403688   1% /run/user/121
# tmpfs             403716        28    403688   1% /run/user/1000

# show file system disk space usage in human readable format
df -h
# Filesystem      Size  Used Avail Use% Mounted on
# udev            2,0G     0  2,0G   0% /dev
# tmpfs           395M  1,6M  393M   1% /run
# /dev/sda1       9,8G  5,7G  3,6G  62% /
# tmpfs           2,0G   98M  1,9G   5% /dev/shm
# tmpfs           5,0M  4,0K  5,0M   1% /run/lock
# tmpfs           2,0G     0  2,0G   0% /sys/fs/cgroup
# tmpfs           395M   28K  395M   1% /run/user/121
# tmpfs           395M   28K  395M   1% /run/user/1000

# show disk usage of directories in '/usr' directory
du /usr
# 15596   /usr/sbin
# 4       /usr/local/sbin
# ..
# 1432    /usr/libexec
# 2988292 /usr

# show disk usage summary of '/usr' directory
du /usr -s
# 2988292 /usr

# show disk usage of directories in '/usr' directory with a depth of zero
du /usr -d0
# 2988292 /usr

# show disk usage of directories in '/usr' directory with a depth of one
du /usr -d1
# 15596   /usr/sbin
# 64      /usr/local
# ..
# 1432    /usr/libexec
# 2988292 /usr

# show disk usage in human readable form
du /usr -d1 -h
# 16M     /usr/sbin
# 64K     /usr/local
# ..
# 1,4M    /usr/libexec
# 2,9G    /usr

# show disk usage of '/usr' and '/lib' directories
du /usr /lib -d1 -h
# 16M     /usr/sbin
# 64K     /usr/local
# ..
# 1,4M    /usr/libexec
# 2,9G    /usr
# 12K     /lib/linux-sound-base
# 16K     /lib/hdparm
# ..
# 16K     /lib/apparmor
# 836M    /lib

# show disk usage of '/usr' and '/lib' directories with a total
du /usr /lib -d1 -h -c
# 16M     /usr/sbin
# 64K     /usr/local
# ..
# 1,4M    /usr/libexec
# 2,9G    /usr
# 12K     /lib/linux-sound-base
# 16K     /lib/hdparm
# ..
# 16K     /lib/apparmor
# 836M    /lib
# 3,7G    total

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## PIPES AND REDIRECTION
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# tee (1) - read from standard input and write to standard output and files
# echo (1) - display a line of text
# printf (1) - format and print data
# strings (1) - print the strings of printable characters in files.

# Pipes and Redirection
# ~~~
# Every process has at least 3 communication channels
#   Standard Input (STDIN)
#   Standard Output (STDOUT)
#   Standard Error (STDERR)
# Unix has file descriptors for these channels as 0, 1, and 2
# Most commands reads from STDIN and outputs to STDOUT conventionally
# `<`, `>`, and `>>` are used to redirect these channels from/to files
# `|` is used to connect STDOUT of one process to STDIN of another

# Special Device Files
# ~~~
# `/dev/null` accepts and discards all input (returns EOF)
# `/dev/zero` accepts and discards all input (returns NUL)
# `/dev/full` returns NUL bytes when read and ENOSPC when written
# `/dev/random` and `/dev/urandom` produces pseudo-random numbers
# `/dev/tty` refers to the underlying terminal
# `/dev/stdin` refers to the STDIN of the current process
# `/dev/stdout` refers to the STDOUT of the current process
# `/dev/stderr` refers to the STDERR of the current process

# create 'streams.c' file with content up to 'EOF'
cat << 'EOF' > streams.c
#include <stdio.h>

int main(void)
{
	fputs("this text goes to STDOUT\n", stdout);
	fputs("this text goes to STDERR\n", stderr);
	return 0;
}
EOF
# (creates the file)

# compile 'streams.c' source into 'streams' binary
gcc streams.c -o streams
# (compiles the file)

# run 'streams' binary
./streams
# this text goes to STDOUT
# this text goes to STDERR

# run 'streams' binary and discard its output
./streams > /dev/null
# this text goes to STDERR

# run 'streams' binary and discard its output
./streams 1> /dev/null
# this text goes to STDERR

# run 'streams' binary and discard its errors
./streams 2> /dev/null
# this text goes to STDOUT

# run 'streams' binary and discard its output and errors
./streams &> /dev/null
# (no output)

# run 'streams' binary, discard its output, and redirect 2nd channel to 1st
./streams > /dev/null 2>&1
# (no output)

# print 5th line in a file (you should better use sed (1))
cat streams.c | head -n 5 | tail -n 1
#        fputs("this text goes to STDOUT\n", stdout);

# create a random string of specified length (you should better use mkpasswd (1))
cat /dev/urandom | strings -n 1 | tr -d '[:space:]' | head -c 30
# 8On$a?rOY_yKJ-:><h5}d0o2D%6+=W

# download 'Alice in Wonderlands' book in text format and save as 'alice.txt'
wget http://www.gutenberg.org/files/11/11-0.txt -O alice.txt
# (downloads the file)

# word frequency challenge (http://dl.acm.org/citation.cfm?id=5948.315654)
cat alice.txt | tr -cs A-Za-z '\n' | tr A-Z a-z | sort | uniq -c | sort -rn | head -n 5
#    1818 the
#     940 and
#     809 to
#     690 a
#     631 of

# run 'streams' binary and save its output in 'output.txt' file
./streams | tee output.txt
# this text goes to STDERR
# this text goes to STDOUT

# display 'output.txt' file
cat output.txt
# this text goes to STDOUT

# run 'streams' binary, redirect its 2nd channel to 1st, and save its output in 'output.txt' file
./streams 2>&1 | tee output.txt
# this text goes to STDERR
# this text goes to STDOUT

# display 'output.txt' file
cat output.txt
# this text goes to STDERR
# this text goes to STDOUT

# run 'streams' binary, redirect its 2nd channel to 1st, and append its output to 'output.txt' file
./streams 2>&1 | tee -a output.txt
# this text goes to STDERR
# this text goes to STDOUT

# display 'output.txt' file
cat output.txt
# this text goes to STDERR
# this text goes to STDOUT
# this text goes to STDERR
# this text goes to STDOUT

# remove 'output.txt' file
rm output.txt
# (removes the file)

# Exercise
# ~~~
# Create a file containing a list of directories under root.

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## USERS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# su (1) - change user ID or become superuser
# sudo (8) - execute a command as another user
# users (1) - print the user names of users currently logged in
# w (1) - Show who is logged on and what they are doing.
# who (1) - show who is logged on
# whoami (1) - print effective userid
# useradd (8) - create a new user or update default new user information
# usermod (8) - modify a user account
# userdel (8) - delete a user account and related files
# passwd (1) - change user password
# chsh (1) - change login shell

# Users
# ~~~
# Could be real (e.g. a person) or pseudo (e.g. a service)
# Determined by a User Identification Number (UID)
# Accounts stored in `/etc/passwd`
# Passwords stored in `/etc/shadow`

# Exercise
# ~~~
# Create a new user, change its password and set an appropriate login shell.

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## GROUPS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# id (1) - print real and effective user and group IDs
# groups (1) - print the groups a user is in
# groupadd (8) - create a new group
# groupmod (8) - modify a group definition on the system
# groupdel (8) - delete a group

# Groups
# ~~~
# Determined by a Group Identification Number (GID)
# A user can belong to multiple groups
# Each user has also a group with the same name as the user name
# Groups stored in `/etc/group`
# Group passwords stored in `/etc/gshadow`

# Exercise
# ~~~
# Create a new group (e.g. `cmpe`) and add the previously created user to it.

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## FILES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# chown (1) - change file owner and group
# chgrp (1) - change group ownership
# chmod (1) - change file mode bits
# umask - display or set file mode mask (shell builtin)

# Files
# ~~~
# Everything is a file (UNIX legacy)
# Commands are executable files
# System privilege and permissions are controlled with files
# Device I/O and File I/O uses files (except at the lowest level)
# Interprocess communication uses files (UNIX pipes)
# All disks are combined as a single tree under root (denoted with `/`)
# File accesses are handled with ownership and protection
# Each file has an owner user and an owner group

# File Permissions
# ~~~
# First bit denotes the type (e.g. file/dir)
# For each file there are 3 access sets
#   User access (u)
#   Group access (g)
#   Other access (o)
# For each access set there are 3 access types
#   Read (r)
#   Write (w)
#   Execute (x)
# In total there are 9 permissions bits
# There are 3 more bits reserved for special purposes (setuid, setgid, sticky)

# Exercise
# ~~~
# What is the numeric file mode equivalent of the following accesses?
#   rwxr-xr--
#   --x-w--wx
#   -r-rw-x-w (is there anything wrong with this example?)

# Exercise
# ~~~
# What is the accesses equivalent of the following numeric file modes?
#   755
#   644
#   325

# Exercise
# ~~~
# Change the owner and/or group of files/dirs and try permissions.

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## SEARCHING
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# grep (1) - print lines matching a pattern
# find (1) - search for files in a directory hierarchy
# which (1) - locate a command
# locate (1) - find files by name
# whereis (1) - locate the binary, source, and manual page files for a command

# print the lines containing the pattern 'rabbit' in 'alice.txt' file
grep rabbit alice.txt
# never before seen a rabbit with either a waistcoat-pocket, or a watch
# rabbit-hole under the hedge.
# The rabbit-hole went straight on like a tunnel for some way, and then
# a rabbit! I suppose Dinah’ll be sending me on messages next!’ And she
# rabbits. I almost wish I hadn’t gone down that rabbit-hole--and yet--and

# print the lines containing the pattern 'rabbit' in 'alice.txt' file ignoring case
grep -i rabbit alice.txt
# CHAPTER I. Down the Rabbit-Hole
# picking the daisies, when suddenly a White Rabbit with pink eyes ran
# ..
# These were the verses the White Rabbit read:--
# The long grass rustled at her feet as the White Rabbit hurried by--the

# print the lines not containing the pattern 'rabbit' in 'alice.txt' file
grep -v rabbit alice.txt
# Project Gutenberg’s Alice’s Adventures in Wonderland, by Lewis Carroll
#
# ..
# Archive Foundation, how to help produce our new eBooks, and how to
# subscribe to our email newsletter to hear about new eBooks.

# print the lines containing the pattern 'rabbit' recursively for all files in the current directory
grep rabbit -r .
# ./basics.sh:# print the lines containing the pattern 'rabbit' in 'alice.txt' file
# ./basics.sh:grep rabbit alice.txt
# ..
# ./alice.txt:a rabbit! I suppose Dinah’ll be sending me on messages next!’ And she
# ./alice.txt:rabbits. I almost wish I hadn’t gone down that rabbit-hole--and yet--and

# search all files in the current directory
find .
# .
# ./streams
# ./basics.sh
# ./streams.c
# ./alice.txt
# ./demo
# ./demo/bar.txt
# ./demo/foo.txt
# ./hello
# ./hello.c

find . -name '*.c'
# ./streams.c
# ./hello.c

# search all executable files in the current directory
find . -executable
# .
# ./streams
# ./demo
# ./hello

# search all executable regular files in the current directory
find . -executable -type f
# ./streams
# ./hello

# find the longest header files under '/usr/include'
find /usr/include -name '*.h' -type f -exec wc -l {} \; | sort -rn | head
# 6706 /usr/include/c++/7/bits/basic_string.h
# 6005 /usr/include/c++/7/bits/random.h
# 5836 /usr/include/c++/7/bits/stl_algo.h
# 5470 /usr/include/linux/nl80211.h
# 4106 /usr/include/reglib/nl80211.h
# 3789 /usr/include/elf.h
# 2967 /usr/include/c++/7/ext/vstring.h
# 2844 /usr/include/c++/7/bits/regex.h
# 2653 /usr/include/c++/7/bits/locale_facets.h
# 2614 /usr/include/c++/7/bits/stl_tree.h

# find the total line of C code in the current directory
find . -name '*.[ch]' -type f -exec wc -l {} +
#   8 ./streams.c
#   7 ./hello.c
#  15 total

# create a soft link 'alive' to 'alice.txt'
ln -s alice.txt alive
# (creates the link)

# show if the 'alive' file exists or not
test -e alive && echo 'file exists' || echo 'file does not exists'
# file exists

# create a soft link named 'dead' to a non-existent file
ln -s /path/to/non/existent/file dead
# (creates the link)

# show if the 'dead' file exists or not
test -e dead && echo 'file exists' || echo 'file does not exists'
# file does not exists

find . -type l
# ./dead
# ./alive

find . -type l -print
# ./dead
# ./alive

find . -type l -print -print
# ./dead
# ./dead
# ./alive
# ./alive

# find existing links in current directory
find . -type l -exec test -e {} \; -print
# ./alive

# find broken links in current directory
find . -type l -! -exec test -e {} \; -print
# ./dead

# remove 'dead' and 'alive' links
rm dead alive
# (removes the links)

# locate 'ls' command
which ls
# /bin/ls

# locate 'gcc' command
which gcc
# /usr/local/bin/gcc

# locate all 'gcc' commands in the 'PATH' variable
which -a gcc
# /usr/local/bin/gcc
# /usr/bin/gcc

locate stdio.h
# /usr/include/stdio.h
# /usr/include/c++/7/tr1/stdio.h
# /usr/include/x86_64-linux-gnu/bits/stdio.h
# /usr/lib/x86_64-linux-gnu/perl/5.26.1/CORE/nostdio.h

whereis gcc
# gcc: /usr/bin/gcc /usr/lib/gcc /usr/share/man/man1/gcc.1.gz

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## COMMANDS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Commands
# ~~~
# An executable file in `$PATH`
# A shell builtin (implemented either as necessity or performance)
# An alias (defined with `alias` builtin)
# Relative/Absolute path to an executable file

# show 'PATH' variable
echo $PATH
# /home/gokcehan/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

# show directories in 'PATH' variable line by line
echo $PATH | tr : '\n'
# /home/gokcehan/.local/bin
# /usr/local/sbin
# /usr/local/bin
# /usr/sbin
# /usr/bin
# /sbin
# /bin
# /usr/games
# /usr/local/games
# /snap/bin

# show the type of 'mkdir' command
type mkdir
# mkdir is /bin/mkdir

# show all types of 'cd' command
type -a cd
# cd is a shell builtin

# show all types of 'echo' command
type -a echo
# echo is a shell builtin
# echo is /bin/echo

# show the type of 'grep' command
type grep
# grep is aliased to `grep --color=auto'

# show the type of './hello' command
type ./hello
# ./hello is ./hello

# create 'cat.c' file with content up to 'EOF' (from K&R)
cat << 'EOF' > cat.c
#include <stdio.h>

main()
{
	int c;
	while ((c = getchar()) != EOF)
		putchar(c);
	return 0;
}
EOF
# (creates the file)

# compile 'cat.c' source into 'cat' binary
gcc cat.c -o cat
# (compiles the file)

# copy 'alice.txt' file to 'alice.txt.bak'
./cat < alice.txt > alice.txt.bak
# (copies the file)

# compare the contents of 'alice.txt' and 'alice.txt.bak'
cmp alice.txt alice.txt.bak
# (no output -- files are identical)

# remove 'alice.txt.bak' file
rm alice.txt.bak
# (removes the file)

# create 'echo.c' file with content up to 'EOF' (from K&R)
cat << 'EOF' > echo.c
#include <stdio.h>

main(int argc, char *argv[])
{
    while (--argc > 0)
        printf("%s%s", *++argv, (argc > 1) ? " " : "");
    printf("\n");
}
EOF
# (creates the file)

# compile 'echo.c' source into 'echo' binary
gcc echo.c -o echo
# (compiles the file)

# run 'echo' binary with arguments 'hello' and 'world'
./echo hello world
# hello world

# create 'shell.c' file with content up to 'EOF'
cat << 'EOF' > shell.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/wait.h>

#define MAXLINE 1000
#define MAXARGS 64
#define DELIMITERS " \f\n\r\t\v"

int main(void)
{
	char line[MAXLINE];

	size_t ind;
	char *tok;
	char *toks[MAXARGS+1];

	while (1) {
		printf("shell> ");
		if (fgets(line, MAXLINE, stdin) == NULL) { break; }
		ind = 0;
		tok = strtok(line, DELIMITERS);
		while (tok != NULL) {
			toks[ind++] = tok;
			tok = strtok(NULL, DELIMITERS);
		}
		toks[ind] = NULL;
		if (toks[0] == NULL) { continue; }
		if (strcmp(toks[0], "cd") == 0) {
			chdir(toks[1] ? toks[1] : getenv("HOME"));
			continue;
		}
		if (fork() == 0) { exit(execvp(toks[0], toks)); }
		wait(NULL);
	}

	return 0;
}
EOF
# (creates the file)

# compile 'shell.c' source into 'shell' binary
gcc shell.c -o shell
# (compiles the file)

# run and interact with 'shell' binary
./shell
# shell> ls /usr
# bin  games  include  lib  libexec  local  sbin  share  src
# shell> ls /path/to/non/existent/file
# ls: cannot access '/path/to/non/existent/file': No such file or directory
# shell> pwd
# /home/gokcehan/cmpe230/basics
# shell> cd ..
# shell> pwd
# /home/gokcehan/cmpe230
# shell> cd basics
# shell> pwd
# /home/gokcehan/cmpe230/basics
# shell> echo hello world
# hello world

# create an alias named 'loc' with an argument at the end
alias loc="find . -name '*.[ch]' -type f -exec wc -l {} + | sort -rn | head -n"
# (defines the alias)

# run 'loc' alias with the argument '5'
loc 5
#   71 total
#   39 ./shell.c
#    9 ./cat.c
#    8 ./streams.c
#    8 ./echo.c

# remove alias 'loc'
unalias loc
# (removes the alias)

# create a function named 'loc' with an argument at an arbitrary location
loc() { find "$1" -name '*.[ch]' -type f -exec wc -l {} + | sort -rn | head; }
# (defines the function)

loc .
#   71 total
#   39 ./shell.c
#    9 ./cat.c
#    8 ./streams.c
#    8 ./echo.c
#    7 ./hello.c

loc /usr/include/
#   361341 total
#     6706 /usr/include/c++/7/bits/basic_string.h
#     6005 /usr/include/c++/7/bits/random.h
#     5836 /usr/include/c++/7/bits/stl_algo.h
#     5470 /usr/include/linux/nl80211.h
#     4106 /usr/include/reglib/nl80211.h
#     3789 /usr/include/elf.h
#     2967 /usr/include/c++/7/ext/vstring.h
#     2844 /usr/include/c++/7/bits/regex.h
#     2653 /usr/include/c++/7/bits/locale_facets.h

# Exercise
# ~~~
# Echo the words `hello` and `world` with more than one space between them.

# Exercise
# ~~~
# Improve our rudimentary shell 'shell.c' by adding more features.
# Some of the things you can try:
#   Show current directory in the prompt line
#   Handle quoting for arguments (e.g. `touch "fname with spaces"` should work)
#   Tilde expansion (e.g. `cd ~` should change to the home folder)
#   Parameter expansion (e.g. `cd $HOME` should change to the home folder)
#   (Advanced) Signal handling (see `man signal`)
#   (Advanced) Pipes and redirection

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## ARCHIVES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# tar (1) - The GNU version of the tar archiving utility
# unzip (1) - list, test and extract compressed files in a ZIP archive
# unrar-nonfree (1) - extract files from rar archives
# 7z (1) - A file archiver with highest compression ratio

# create a tar file 'src.tar' from c files
tar cf src.tar *.c
# (creates the file)

# list contents of 'src.tar' file
tar tf src.tar
# cat.c
# echo.c
# hello.c
# shell.c
# streams.c

# extract 'src.tar' file
tar xf src.tar
# (extracts the file)

# create a gunzipped tar file 'src.tar.gz' from c files
tar czf src.tar.gz *.c
# (creates the file)

# uncompress and extract 'src.tar.gz' file
tar xzf src.tar.gz
# (extracts the file)

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## PACKAGES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# apt (8) - command-line interface
# apt-get (8) - APT package handling utility - - command-line interface
# apt-cache (8) - query the APT cache
# dpkg (1) - package manager for Debian
# rpm (8) - RPM Package Manager
# yum (8) - Yellowdog Updater Modified

# Packages
# ~~~
# Software distribution in archive files
# Collected in repositories by distributions
# Comes as either pre-built binary (usually) or source code forms
# Handled by package managers:
#   Installation
#   Upgrading
#   Configuring
#   Removal

# search packages containing the word 'wget'
apt search wget
apt-cache search wget

# show information about 'wget' package
apt show wget
apt-cache show wget

# install 'wget' package
sudo apt install wget
sudo apt-get install wget

# remove 'wget' package
sudo apt remove wget
sudo apt-get remove wget

# update list of packages
sudo apt update
sudo apt-get update

# upgrade all installed packages
sudo apt upgrade
sudo apt-get upgrade

# list installed files of 'wget' package
dpkg -L wget
# /.
# /etc
# /etc/wgetrc
# /usr
# /usr/bin
# /usr/bin/wget
# /usr/share
# /usr/share/doc
# /usr/share/doc/wget
# /usr/share/doc/wget/AUTHORS
# /usr/share/doc/wget/MAILING-LIST
# /usr/share/doc/wget/NEWS.gz
# /usr/share/doc/wget/README
# /usr/share/doc/wget/changelog.Debian.gz
# /usr/share/doc/wget/copyright
# /usr/share/info
# /usr/share/info/wget.info.gz
# /usr/share/man
# /usr/share/man/man1
# /usr/share/man/man1/wget.1.gz

# find the owner package of a file
dpkg-query -S /bin/ls
# coreutils: /bin/ls

# Manual Installation
# ~~~
# A package might not be available in the distro’s repositories
# The package version in the repositories might be outdated
# Traditionally installed under `/usr/local` or `/opt`
# Most packages use the convention `./configure; make; make install`

# Exercise
# ~~~
# Install a package manually.

```

Related:

* <https://gokcehan.github.io/>

Tags:
    
    #linux #bash #posix #commands #convention
