# tmux cheatsheet

* list all sessions and select interactive: `C-a s`
* list all sessions: `C-a :ls` || `tmux list-sessions` || `tmux ls`
* switch to previous session: `C-a L` || `tmux switch-client -l`
* detach current session: `C-a d` ||
* create new session within running session: `C-a :new`


* create a new sessions with name 'foo', or if session already exist attach 'foo': `tmux new -As foo`



