# tmux cheatsheet

## Sessions

* create a new sessions with name 'foo', or if session already exist attach 'foo': `tmux new -As foo`
* create new session within running session: `C-a :new`

* list all sessions and windows, select interactive: `C-a w`
* list all sessions, select interactive: `C-a s`
* list all sessions: `C-a :ls` || `tmux list-sessions` || `tmux ls`
* switch between current&previous session: `C-a L` || `tmux switch-client -l`
* detach current/active session: `C-a d` || `tmux detach`
* kill current/active window and later session: `C-d`
* kill current/active session: `C-a k` || `tmux kill-session`
* kill current/active window: `C-a x` || `tmux kill-window`
* rename window: `C-a ,` || `tmux rename-window`
* rename session: `C-a $` || `tmux rename-session`
* move to previous session: `C-a (` || `tmux switch-client -p`
* move to next session: `C-a )` || `tmux switch-client -n`

