# Troubleshooting vimrc

> 🧐 If you ever wonder why some settings don't apply or what causes it to get
overwritten. Use `:verbose settingName`

E.g I had this annoying fold column on the left side but only in markdown
files. I tried to use `set foldcolumn=0` but that did not apply if I had it as
a setting in vimrc. However it did work if I just used the command in vim
directly like so `:set foldcolumn=0`. So I knew something was overwriting this
setting. Since the vimrc gets read from top to bottom the place where you write
settings matters. So I tried to do that but even if `set foldcolumn=0` it would
not work. But there is a real good solution to understand why settings don't
apply: `:verbose set foldcolumn`. If you wonder the problem was the
`vim-pandoc` plugin. Next best thing is to RTFM! `:help vim-pandoc`

**tldr** if a setting is not behaving the way you'd expect use `:verbose
nameOfSetting`

Tags:

    #vim #linux #ide
