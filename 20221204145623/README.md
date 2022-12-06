# tmux cheatsheet

## General

* show commands: `C-a ?`
* reload/source tmux config: `C-a r`
* enter command mode: `C-a :`

## Sessions

* create a new sessions with name 'foo', or if session already exist attach 'foo': `tmux new -As foo`

* list all sessions and windows, select interactive: `C-a w`
* list all sessions, select interactive: `C-a s`
* list all sessions: `C-a :ls` || `tmux list-sessions` || `tmux ls`
* switch between current&previous session: `C-a L` || `tmux switch-client -l`
* detach current/active session: `C-a d` || `tmux detach`
* attach session: `tmux a -t foo`
* kill current/active session: `C-a X` || `tmux kill-session -t foo`
* kill server: `C-a C-x`
* rename session: `C-a $` || `tmux rename-session`
* move to previous session: `C-a (` || `tmux switch-client -p`
* move to next session: `C-a )` || `tmux switch-client -n`
* create new session: `C-a C-o` || `tmux new -As foo`
* create new session within running session: `C-a C-o` || `C-a :new`

## Windows/Panes

* kill current/active window and later session: `C-d`
* kill current/active pane/window: `C-a x` || `tmux kill-window`
* create new window: `C-a c`
* create new horizontal pane: `C-a |` || `tmux split-window -h`
* create new vertical pane: `C-a -` || `tmux split-window -v`
* rename window: `C-a ,` || `tmux rename-window`
* goto window: `C-a <0-9>`

## Copy Mode

* enter copy mode: `C-a [`
* select text: `v` || `Spacebar`
* copy text: `y` || `Enter`
* paste text: `C-a p` || `C-a ]`
