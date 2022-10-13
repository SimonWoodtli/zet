# What is the difference between eval and exec

They both are use to exec and run commands. However the main difference is 
that `exec` will replace the currents shells process with the given command.
So the current shells PID will become the PID of the command to be executed. 
On the other hand `eval` only run cmds so `eval foo` is the same as `foo`.
However it can execute commands that are stored in a variable.

Related:

* <https://unix.stackexchange.com/questions/296838/whats-the-difference-between-eval-and-exec>

Tags:

    #linux #commands #exec #eval #unix #
