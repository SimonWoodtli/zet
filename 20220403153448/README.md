# Linux Process management

```sh
#!/bin/sh

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## PROCESSES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Unix Processes
# ~~~
# Single executable running in its own address space
# Unix jobs or commands may be composed of multiple processes
# Interactive processes can run in either the background or the foreground
#   `&` is used to run a command in the background
#   ^Z stops foreground process
#   ^C kill foreground process
# Batch processes are submitted to a queue for execution
# Daemons are server processes running continuously while system is up

# Process Attributes
# ~~~
# Process ID (PID)
# Parent process ID (PPID)
# Nice number
# TTY
# Real and effective user ID (RUID, EUID)
# Real and effective group ID (RGID, EGID)

# jobs - lists the active jobs (shell builtin)
# fg - move job to the background (shell builtin)
# bg - move job to the foreground (shell builtin)
# disown - remove jobs from current shell (shell builtin)
# ps (1) - report a snapshot of the current processes
# pstree (1) - display a tree of processes
# pgrep (1) - look up or signal processes based on name and other attributes
# top (1) - display Linux processes
# signal (7) - overview of signals
# kill (1) - send a signal to a process
# killall (1) - kill processes by name
# pkill (1) - look up or signal processes based on name and other attributes

# create 'loop.c' file with content up to 'EOF'
cat << 'EOF' > loop.c
#include <stdio.h>
#include <unistd.h>

int main(void)
{
	int curr = 0;
	while (1) {
		printf("running for %d seconds\n", curr++);
		sleep(1);
	}
	return 0;
}
EOF
# (creates the file)

# compile 'loop.c' source into 'loop' binary
gcc loop.c -o loop
# (compiles the file)

# run 'loop' binary -- interrupt with Ctrl-c to quit
./loop
# running for 0 seconds
# running for 1 seconds
# running for 2 seconds
# running for 3 seconds
# ^C

# run 'loop' binary -- interrupt with Ctrl-z to stop
./loop
# running for 0 seconds
# running for 1 seconds
# running for 2 seconds
# running for 3 seconds
# ^Z
# [1]+  Stopped                 ./loop

# display status of jobs
jobs
# [1]+  Stopped                 ./loop

# move the job to the foreground and then stop again afterwards
fg
# ./loop
# running for 4 seconds
# running for 5 seconds
# running for 6 seconds
# ^Z
# [1]+  Stopped                 ./loop

# move the job to the background
bg
# [1]+ ./loop &
# gokcehan@gokcehan-VirtualBox:~/cmpe230/process$ running for 7 seconds
# running for 8 seconds
# running for 9 seconds
# running for 10 seconds

# abondons the last job
disown
# (job is abondoned)

# abondons the last job with nohup
disown -h
# (job is abondoned with nohup)

# abondons running jobs
disown -r
# (running jobs are abondoned)

# abondons all jobs
disown -a
# (all jobs are abondoned)

# shows all processes in the current terminal
ps
# PID TTY          TIME CMD
# 20348 pts/2    00:00:00 bash
# 20359 pts/2    00:00:00 ps

# shows all processes attached to a terminal
ps -a
#   PID TTY          TIME CMD
#  1077 tty1     00:00:00 gnome-session-b
#  1083 tty1     00:00:12 gnome-shell
# ...
# 19965 pts/1    00:00:01 vi
# 20364 pts/2    00:00:00 ps

# shows all processes
ps -e
#   PID TTY          TIME CMD
#     1 ?        00:00:02 systemd
#     2 ?        00:00:00 kthreadd
# ...
# 20348 pts/2    00:00:00 bash
# 20400 pts/2    00:00:00 ps

# display a tree of processes
pstree
# systemd─┬─ModemManager───2*[{ModemManager}]
#         ├─NetworkManager─┬─dhclient
#         │                └─2*[{NetworkManager}]
# ...
#         ├─whoopsie───2*[{whoopsie}]
#         └─wpa_supplicant

# create 'lock.c' file with content up to 'EOF'
cat << 'EOF' > lock.c
int main(void)
{
	int curr = 0;
	while (1) {
		++curr;
	}
	return 0;
}
EOF
# (creates the file)

# compile 'lock.c' source into 'lock' binary
gcc lock.c -o lock
# (compiles the file)

# run 'lock' binary -- interrupt with Ctrl-c to quit
./lock
# (lock is running)

# show resource usage of all processes
top
# (displays processes in top -- press 'q' to quit)

# shows resource usage of all processes owned by gokcehan
top -u gokcehan
# (displays processes in top -- press 'q' to quit)

# lists all processes with the name loop
pgrep lock
# 33
# 20463

# kills process with id 1234
kill 1234
# (SIGTERM is sent to the process)

# force kills process with id 1234
kill -9 1234
# (SIGKILL is sent to the process)

# kills all processes with the name lock
pkill lock
# (SIGTERM is sent to the processes)

# force kills all processes with the name lock
pkill -9 lock
# (SIGKILL is sent to the processes)

# kills all processes with the exact name lock
killall lock
# (SIGTERM is sent to the processes)

# force kills all processes with the exact name lock
killall -9 lock
# (SIGKILL is sent to the processes)

# Exercise
# ~~~
# Start a process in the foreground and then move to background
# Find a way to make the process continue after closing the terminal
# List all processes of a user and sort by
#   Cpu usage
#   Memory usage
# Create a cpu intensive process and then kill it

```

Related:

* <https://gokcehan.github.io/>

Tags:
    
    #linux #process #commands #processManagement
