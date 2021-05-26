# bboost20: Day 15

Linux Beginner Boost - Day 15 (May 22, 2020)

##  Wednesday, May 22, 2020, 01:27:25PM

### Build your VIMrc

**HINTS**
* make a vim dir (same structure as rob on gitlab) and then you can write for each program a tiny setup script
* you should make a separation for your dotfiles. stuff you want to be public and stuff you want to keep private (like a spelling dictionary?) private logs too.
```
set rtp^=~/.vimpersonal
set rtp^=~/.vimprivate
```
* vimrc is written in vimscript
* to reload your vimrc settings while you edit them: `:so %`
* if you want to see all the colorschemes: `:colorscheme TAB-TAB-TAB-TAB`
* `:set list` will show you the formation and the structure you typed. `set nolist` to go back to normal
* a swap file is: when you are editing a file it will create a file with the name of the file + a tilde ~. It sometimes also makes backup incase you have to recover.
* the vim8 builtin plugin manager sucks: because it does not mash well with a dotfiles cloud directory (won't download any plugins automatically, and none of the existing plugins use the architecture)
* if you paste stuff from vim and wanna make sure the code is with the same indentation use `:set paste` before you go `i`

---

### General Notes

* bbb

#### Common Practice

1. bb

#### Tips

1. bb
1. you cant say you are a programmer unless you know at least: 1 compile language, 1-2 interpreted language.
1. python has significant whitespace= bad programming design decision, haskell did this too and nim
1. haskell over lisp more widly used
