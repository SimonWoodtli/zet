# Clipboard over SSH with Vim

Xclip will install a couple of x11 dependencies. If that is already considered bloat on your server don't use this.

1. install xclip on local machine and server
2. enable X11 forwarding on the server: add line 'X11Forwarding yes' to '/etc/ssh/sshd_config'
3. make sure you have xclip key mapping on your servers .vimrc 

```vim
let mapleader ="\<Space>"
vmap <leader>yy :!xclip -f -sel clip<CR>
map <leader>pp mz: -1r !xclip -o -sel clip<CR>`z
```

4. Either always the -X flag when you ssh into server: `ssh -X foo@foo`
Or also add ‘X11Forwarding yes’ to '/etc/ssh/sshd_config’
