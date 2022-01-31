# TMUX panes no boarder (transparent terminal)

compile from source to be able to have no borders in your tmux panes.

Get tmux from github and compile from source but first change the "screen-redraw.c" file.

 18 # In screen-redraw.c change this:
 19 #define CELL_BORDERS " xqlkmjwvtun~"
 20 # To this:
 21 #define CELL_BORDERS "
