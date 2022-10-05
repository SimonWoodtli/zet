# Move a running process into a tmux session

Click [here] to see the GitHub repo

1. Install [reptyr]
1. Suspend the respective process with Ctrl-Z
1. Send the job to background using bg
1. Take away the ownership from the shell using disown
1. Start or enter your tmux/screen session
1. Run reptyr PID to attach the process to the current shell

[reptyr]<https://pkgs.org/search/?q=reptyr>
[here]<https://github.com/nelhage/reptyr>
