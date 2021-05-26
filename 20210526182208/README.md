# bboost20: Day 3

Linux Beginner Boost - Day 3 (May 6, 2020)

##  Wednesday, May 06, 2020, 01:18:49PM

### Terminal Basics

***HINTS***
* tripple mouse click to select whole line
* `$ touch filename` creates file.
* `CTRL-C` to quit current process and `CTRL-D` to quit Terminal. If you want to suspend use CTRL-S and CTRL-X puts in in the background (I guess).

### VIM

***HINTS***
* VIM has 2 modes: Insert and Command. To insert:`i` edit file at cursor point later save:`CTRL+[ZZ` (capital Z!).
* If you want to have number line enter `set nu` in command-mode.
* To exit insert-mode you have two options either use `Esc` or `CTRL-[`. And to exit vim you can use `:q` save-exit `:wq`. For me `Esc` `ZZ` works best
* If you feel stuck in vim hit Esc couple of times to get back to command-mode.
* save content from a file into new file (save as) `:w filename` (keeps original one open, after saved).
* save content from a file into new file (save as) `:save filename` (original file gets closed and new one opens)
* `sudo update-alternatives --config editor` select vim as default editor

#### General

```
ZQ= exit no save (:q) or :q!
ZZ= save&exit (:wq) or :x
```

**Navigate**
* in insert-mode: Ctrl-x then C-f for file completion e.g. if you wanna write a path
```
h,j,k,l
```
VI-mode in terminal: use Esc or `C-[` to get into vi-mode
VI-mode in terminal: use "s" to get out of it and delete 1 character at cursor
VI-mode in terminal: use "C" to get out of it with a clean line

#### Commands: letters

***HINTS***
* Commands always target @cursor
* Difference between "a" and "i"? i=insert@cursor a=insert after cursor
* Commands are made of an operator and a motion dw, d=operator w=motion
i=insert at cursor I= Insert at beginning of line
a=insert after cursor A= Insert at end of line
.= use the last used command (useful)
if you want to change case lower to upper or other way: in command-mode
use `~` over the letter you wanna change.

**delete** (actually is cut)
```
x= delete 1letter
dw= delete 1word
d2w= delete 2word
d$= delete end of line
D= delte to end of line
de= delete
dd= delete 1 whole line
2dd= delete 2 lines
d}= delete paragraph (section)
dG= delete until end of file
dt"= delte until next " shows up. (works for any letter)
d:30= it will delete from cursor to line 30
ce/cw= delete word +insert
c$/C= delete to end of line +insert
d3l= delete 3 characters
c3l= delete 3 character +insert
```
**navigate**
```
2w= skip 2 words to beginning
3e= skip 3 word to ending
O= go to start of of line
$= go to end of line
CTRL-G= see current line
gg/:0= start of file
G/:$= end of file
22G/:22= go to line 22
:m#= moves the current line
```
**copy/paste**
```
v=mark text
y=copy
yy= yank 1 line
yw=copy 1word
y3l= yank 3 characters
p=paste
10p=paste same line 10times
u= undo
U= undo for whole line to original
dd followed p= delete and paste
```
r=correct letter e.g. "re" remove letter replace with e
R= replace letter with new one
o= insert on next line
O= insert above line
**search**
/searchterm= search for word "searchterm"
?searchterm= same as / but search upwards
n= next searchterm N= previous searchterm
%= looking for parenting bracket (needs cursor@bracket) `([{`
**important** 1. /searchterm 2. cw and make your changes to your searchterm 3. `n` for the next, if you keep it `n` again 4. if you want the same changes to apply again `.`

#### X-Commands

***HINTS***
* `:` commands always need be confirmed "enter"

`:s/old/new`= replace old with new on line
`:s/old/new/g` = replace whole file
`:!command`= shell command ability e.g `:!ls`= ls command from shell
`:set ic`= ignore CAPITAL if /search search both
`:help` , Ctrl-] will go to the | linked word | always piped

#### Magic Wands:

`!!scriptname`= paste any script content into vim
`!!cat $(which rndcolor)`= paste script into vim (`which rndcolor`= quicker with backticks)

### GIT via Terminal

***HINTS****
* `$ cat .ssh/id_ed25519.pub` -> check if you get output
* `$ xclip < ~/.ssh/id\_ed25519.pub`= same but now in copy/paste buffer
* GIT: A commit is a way to put a checkpoint to what is being saved. And give you a comment about everything you did since the last time you made a change.
* GIT:

#### Generate SSH-Keys

***HINTS***
* If you create an `ssh-key` they always come in pairs. You will get a private and a public key. It is possible to have mutiple keys on Gitlab. You upload the public key to Gitlab and keep the private key on your machine locally. *Never* share your private-key with anyone! For real projects consider using a password (passphrase), skip this by just `enter` trough it.
* \# STEP1: maybe also able to copy old key if you already have one, otherwise just create a new one

STEP1:
\# ed25519: best and safest way to do it as of today, Quantum resistent.
```
`ssh-keygen -t ed25519` to generate a key with ed255 encryption
`cat .ssh/id_ed25519.pub` and save that key on the GITLAB website.
```

#### Gitlab Setup

***HINTS***
* `git add -A` adds everything
  always use `git pull` **first**  to synch down to your local someone else may have
  made changes to the Repos! Then you can make changes and git push them
  back
* If you do however have a **mergeconflict** because you did not pull first.
  \1. `git stash` will remove your changes locally \2. `git pull` \3.
  `git stash pop index` will put your local changes back 4. commit and push
* if you are in a dir/file you think it might be on gitlab check with `ls .git` (you need to be in the right folder!) or `ls -a` if no .git is there your dir is not on Gitlab yet.
* If you make changes to eight ball `git status` will let you know then you have to 1. commit `git commit -m "update eightball"` and 2. push them up `git push` (-u no longer needed)

**Basic Steps for GIT**
1. Initialize it
1. Add stuff
1. Commit it
1. Push it

STEP2: ssh-test
\# if you get permission error make sure your .ssh folder only has keys and no config
```
`ssh git@gitlab.com` -> check if you can connect
`ping gitlab.com`  check if you can reach server
```

STEP3: configure git with your gitlab credentials

```
$ git config --global user.name xnasero
$ git config --global user.email xnasero@protonmail.com
```


STEP4: ADD NEW FOLDER from your PC
\# make local dir git-ready

`git init`

\# make gitlab the master and turn on synch
`git remote add origin git@gitlab.com:xnasero/dotfiles.git`

STEP4.1: PULL EXISTING FOLDER from GITLAB
\# STEP 1,2,3,4,5 are if you are on new PC and create new folder and want to push that
\# IF you are on a new PC and want to pull from GITLAB: STEP 1,2,3,4.1
\# maybe `git init` is also neccessary

```
cd existing-repo
git remote rename origin old-origin
git remote add origin git@gitlab.com:xnasero/dirname.git
git push -u origin --all
git push -u origin --tags
```

If you get current branch master has no upstream branch: `git push -u origin master`


STEP5: first PUSH
```
1. Now you can use `git add .` to add the folder. Quick check `git status`
1. `git commit -m "initial commit"` to commit ("initial commit"=comment). Quick check `git status` (so far your local git is up2date in the next step the other git on gitlab needs to get updated)
1. `git push -u origin master` will push it on gitlab (master=master branch on gitlab)
```

#### GIT CLONE

```
git clone git://github.com/git/hello-world.git
cd hello-world
git fetch
git pull
git fetch & merge (one step for fetch/pull)
```

##### GITLAB Basic commands
``
git config --global user.name "xnasero"
git config --global user.email "xnasero@protonmail.com"
**Create a new repository**
git clone git@gitlab.com:xnasero/dirname.git
cd any
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

**Push an existing folder**
cd existing_folder
git init
git remote add origin git@gitlab.com:xnasero/dirname.git
git add .
git commit -m "Initial commit"
git push -u origin master

**Push an existing Git repository**
cd existing-repo
git remote rename origin old-origin
git remote add origin git@gitlab.com:xnasero/dirname.git
git push -u origin --all
git push -u origin --tags
```

----

### General Notes

* Grok= to fully understand something and internalize it.
* There is a **difference** between CLI and Terminal. They are not the same Interface.
* `less filename.pdf` = view pdf in terminal
* Robs vim-plugins:
vim-pandoc
vim-gitgutter
vim-pandoc-syntax-simple
vim-go
vim-toml
vim-ps1
* vim TMUX command: in vim command-mode press `:swap-pane -s 1 -t 2` I think it is for switching windows
#### Common Practice

1. Always create file `touch` before you edit in vim.
1. Never show your private ssh-key to anyone. Only public .pub one.

#### Tips

1. You can have a .gitconfig for each directory
