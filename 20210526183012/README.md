# bboost20: Day 4

Linux Beginner Boost - Day 4 (May 7, 2020)

##  Wednesday, May 07, 2020, 01:19:27PM

**True Terminal Mastery:** Combination bash+vi+bash vi-mode+lynx configured like vi+tmux configured to use vi bindings+some handwritten shellscripts and aliases.

### Vi in Bash shell

1. `set -o vi` into bashconfig
1. to enter vi-mode in terminal: CTRL-left bracket or Esc
1. hjkl to navigate history
1. /search and n N to search trough history

### LYNX

to search duck: `? searchterm`
`type \?`= its alias for a script called duck (found on robs gitlab)

1. download (gitlab.com/rwxrob config lynx.lss  (color settings etc.) lynx.cfg (config file) put them rwxuser/dotfiles/lynx
1. add the symlink to your setup.sh
```

ln -sf $PWD/lynx/lynx.lss $HOME/.config/lynx/lynx.lss
ls -l $HOME/.config/lynx/lynx.lss

ln -sf $PWD/lynx/lynx.cfg $HOME/.config/lynx/lynx.cfg
ls -l $HOME/.config/lynx/lynx.cfg

```
1. download the duck and google scripts to search (gitlab/rwxrob)
1. put them in your Repos script dir
1. `duck searchterm` `google searchterm` in terminal
1. or create an alias in bashconfig? how?

### Xclip

***HINTS***
* `cal | xclip`= put calendar into your mouseclick paste buffer
* Redirect=`>` vs Pipe=`|`
* Redirect puts collected input into a file. takes the output of that program and puts it into a file
* Pipe puts collected output to selected program via mouseclick. it takes an output of one program and sends it to another program

### dotfile Repo

***HINTS***
* create a Repos in your ~dir and put all the stuff in there that you need
  to get synched.
* **Repos/gitlab.com/ put accounts there like rwxyou** so that dotfiles
  dont get mixed up create multiple accounts!
* avoid hidden files in your Repos for dotfiles and the like
  if you create a .vimrc in home and copy to your Repos/gitlab you need
  to make sure they are linked! Otherwise The changes you make in
  ~/.vimrc wont get into your Repos vimrc and therefor no changes will be
  synched! (symbolic linking)
* put all your config and scripts into dotfiles
* files like bashconfig can become .sh or .bash (same) if you set them `au bufnewfile...` in vimc

----
To run changes in your config files: `exec bash` and ./setup (depends on the scriptname u choose)
----

#### vim configuration

***HINTS***
* .vimrc needs to be created
* .vimrc is filled with ":set ..." commands
* steal .vimrc from other people but be selective. Don't copy everything
use what you know or try to figure out what new things may do.

1. create a .vimrc `touch .vimrc` in home configfile `vim .vimrc` to edit
1. If command-mode `:colorscheme "TAB"` trough different schemes
1. use `set listchars=tab:→\
,eol:↲,nbsp:␣,space:·,trail:·,extends:⟩,precedes:⟨` now :set list shows
formations
**Magic Wands**
They allow you to use your shell for expansion. So you can write a script
and use it within vim. :!alias that is why vim is so powerful. There is
no need for many plugins and extras because you can use scripts for those
tasks instead. Think about what kind of script to write instead what
macro/plugin could be used.

----
1. create /Repos/rwxyou/dotfiles
1. `cd /Repos/gitlab.com/rwxyou/dotfiles`
1. `cp ~/.vimrc .`
1. `mv .vimrc vimrc`
1. also add your a README.md
1. `diff ~/.vimrc vimrc` = no output means they are the exact same file Now make sure those two files are linked so changes apply to both! But first lets save the file on gitlab by pushing it.
1. check file to see what goes into vimrc also learn to customize it by yourself
----

#### setup bashconfig

***HINTS***
* when you first login to a shell: it runs system scripts that setup all
  kinds of things and then it runs the .bashrc script. If its a login
  script it will also run .profile
* first update your .bash"underscore"aliases and if you really get bashrc
  you can put some changes to .bashrc. But if not its better to leave
  .bashrc in default and make your personal changes in .bash-aliases.
  Because you might need to run on a shell on a computer that does not
  allow your custom bashrc but will allow bash-aliases
* .profile is not always executed for bash so you should not use that
  either stick with bash-aliases for personalization. bashprofile only
  runs for login shells. It does not run for all things!
* bash manual says better use functions then aliases. so dont go to crazy
  with them they are meant to be temporary.
* if you want to disable an alias use \command. It "overwrites" the alias
  and uses the original command. e.g. `vi` (runs vim, because you set it
  as an alias) to run vi use \vi.


1. cd Repos/gitlab.com/rwxyou/dotfiles/
1. `touch bashconfig` (sounds better then name it bashaliases)
1. vim bashconfig
1. add `alias ll="ls -la"` `alias vi=vim`
1. sourcing= run it in this case we want `. bashconfig`
1. type vi
1. `which vi` = /usr/bin/vi `ls -l $(which vi)`= check if its a symbolic
link in this case vi is actually already linked to vim. so the alias is
overkill.

**SETUP dircolors**
1. download dircolors from gitlab/rwxrob
1. put it in your Repos/gitlab.com/rwxyou/dotfiles
1. edit your Setup in your Repos and add another softlink to ~/.dircolors
1. add to bashconfig: robs dircolor script

**SETUP cmd-prompt colors**
1. copy command prompt colors from gitlab.com/rwxrob
2. paste it into your bashconfir on your Repos

##### setup bash..aliases

***HINTS***
* `echo $PATH` shows all the bash scripts that run automatically (no ./
  needed)
* `exec bash` runs a new shell=best way to do, also `.
  bashconfig`=`source bashconfig` works
  but use exec) (not sure: do some research)
* .bash and .sh is the same
* every alias you add to bashconfig comes after the unalias line
----
1. make sure bashconfig from gitlab.com is linked to ~/.bash..aliases
how (bash..aliases doesn't exist in clean install)? `ln -sf
$PWD/bashconfig ~/.bash..aliases`
1. create an install script because \1. command is used so often: `touch
setup` put commands in there
1. mkdir scripts and put your scripts in there
1. `exec bash` and `. bashconfig`
1. bashconfig: `unalias -a` will remove old aliases that are no longer in
your config but the shell would still remember
1. `set -o vi` tells you to use a shell history, makes vi commands
h,j,k,l / etc available for your shell
1. activate your bash `. bashconfig` if you add new things redo this
1. rename bashconfig to bashconfig.bash (now its a known filetype to
bash) also: change pathway in the setup file (update ln in it) now run
./setup again THIS METHOD works but its easier not to have fileextensions
in your dotfiles next step will show
1. rename bashconfig.bash back to bashconfig, edit the setup ln pathway,
and add `au bufnewfile,bufRead bashconfig set filetype=sh` to vimconfig
----

### TMUX

***HINTS***
* TMUX= Terminal Multiplexer= a way to split your terminal screen
* Good way to split screen because it works even if you ssh from a Windows Computer into a shell.
* TMUX replaced screen that one was used since 70's
* play around in tmux.config from rob and figure stuff out
* good for sysadmins because reliably working on any machine. (change your Metakey to Ctrl-A instead of default CTRL-B)
* comes not on systems by default
* TMUX can be customized to work so shotcuts and hotkeys work like in vim.

1. Use CTRL-A-| to tab between windows


##### setup ix

***HINTS***
* rob recommends curl over wget. It gives you more control and is more precise and uses the actual libcurl library
1. `curl http://http://ix.io/2iTo > ix` > means store output somewhere.
in this case in the ix dir. (this might not work in future dont know how
long this link is available. However the ix script is also on rwxrob's

to create/upload to ix: `ix < filename`
to download from ix: `ix 2iTo > filename` (quicker than curl)

----

### General Notes

* people make dot file public other people look at it. If they are cool they might be intrested in hiring you.
* why bash? linux comes with it built-in. Linux shell=bash
+ dont use Zshell! standard is bash. It does not support important things.
It is standard shell for mac.
* Dash shell is fast and is symlinked. Works on any UNIX since 70's.
* Dash & Bash are what I need to learn and focus on.
* `exit` to logout only for terminal `sudo su - username` to login with a different account in terminal
* process= running program on the system

#### Common practice

1. create account dirs for dotfile/config stuff in your Repo dir
1. in bash scripts use \$HOME instead of ~
#### Tips
* GIT: dont edit&save files local and cloud and then try to push this!
* don't mess with things on your system unless you have to.
