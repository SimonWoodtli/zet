# bboost20: Day 14

Linux Beginner Boost - Day 14 (May 21, 2020)

##  Wednesday, May 21, 2020, 01:26:55PM

### VI

***HINTS***
* oldest editor on planet except for ed.
* visual mode for ed was developed and became vi=visual mode. X was some editor in between.
* vi is a combination of x-mode x-line editor and visual mode. If you press the wrong button you might end up in the x-mode: press :vi to get back
* why vi? because it is on everything even on windows with linux subsystem on it.
* original vi can still be used it is called `nvi` (also bugs included)
* with pandoc you can convert a markdown .md into a html file with just a few keystrokes all in vim. `!}pandoc` keyword: magic wand commands for vi
* power of vi comes with its close integration of the shell
* when you understand the integration "if you want lines to move in `3>>` will move 3 lines inof vi with the shell the amount of plugins, macros and customisation you need is almost 0. so your customisation of your website can actually be writting shell scripts, or go script or any other script.
* use `:Plug` for your plugin manager the best.
* if :shell you get out of vim to the shell and can do stuff there `exit` to get back to vim
* to paste big chunks in vim into another file: put cursor at beginning of you section `!:34` Enter now remove "w" change to "w" and the place you want: :.,.+15w /tmp/foo (this is how it should look like)
* to insert text from a file in vim: `:r /tmp/foo`
* when you yank a section `y{` you can paste it into another file with `p`. however make sure to have the viminfo line in your vimrc. Otherwise stuff gets deleted because the p buffer can't handle it.
* don't use vim magic wands in the shell. just use them in vim. d
* if you want 3 lines in vim to move in like with tabs `3>>` you can also do `<<`
* `:setlocal spell` to spellcheck in vim

{= move to beginning of paragraph
}= move to end of paragraph

for i in {1..10}; do echo Item \$i.; done \# this line of bash code can be send to the bash program with `!!bash`
which gives this output all within vim: (it will replace the written code)
Item 1.
Item 2.
Item 3.
Item 4.
Item 5.
Item 6.
Item 7.
Item 8.
Item 9.
Item 10.

* to comment: `!}cmt ///` (if you want to comment a  line with /// also `cmt` is robby scrip)
* `!!hnow` will print you a timestamp (and it actually is a shell script from rob). You can use any command and have the output directly in vi!!!

e.g. you have a line like:
This is a title
you can send this title-line with robs `htitle` script to ... with `!!htitle`
O: ############################ This is a Title #############################


this is a [Terminal](https://duck.com/lite?q=Terminal).
this term "Terminal can be searchable on duckduck: you can set this up in vimrc.
If you make an empty hyperlinked name in markdown: \[searchterm\](https://duck.com/lite?q=searchterm) and close vi again it will make it searchable on duckduck

You can even use ix or any other tool that grabs stuff from internet with this magiccards.
1. `ix < ix` take the link http://ix.io/2iTo (just the number at end)
2. lets say you are writing a blog and or a note and want to add this script to it.
3. you can now
```bash
!!ix 2iTo
```
this would create a box with your script that you got via ix (internet). all while still being in vim and not leaving it.

----

### General Notes

* Perl is the most powerful language for regular expressions. (it is the standard for that, included directly into any other language)

#### Common Practice

1. bb

#### Tips

1. `:vi` in vi if you are stuck
1. `:reg` shows you register options: allows you to paste registered stuff and paste it with the given number (super useful). If you want 1 just `1p` and it will paste 1 from your register. You can't use this paste-buffer in different tmux sessions or ssh. So Rob suggests to use tmux builtin copy/paste? how?
1. use `U` to undo all the changes you made in your current insert session. (useful)
1. vi wildcards are super nice to make a blog and get those commands from your shell into your editor in a breeze!
1. use the Esc with your left ring finger while still on f with index finger to get into command-mode, maybe it is more comfortable than Ctrl-\[
1. if you are in vi and you wanna exit and go back again make it a habit to look for a word to search for so you get there. I think looking at the current line is even better.
1. a $ in vim is to the end of a line. It also represents the end of a string in regular expression.
1. MOST IMPORTANT VIM WANDS: line magic wand, section selector magic wand, select to the end of file magic wand. Then you can send those lines to any program you want.
1. you don't need to memorize all the different x-commands in vi. just `!G` and then delete the ! so you don't have to memorize the beginning. Works for all kinds of x-commands.
1. If you want to comment the whole document cursor at begginning then `!G` delete ! add `s/^/#` hit enter
