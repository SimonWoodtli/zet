# LINUX: Frozen Screen - REISUB

Of all command key sequences, reisub is probably the most famous. To better remember this sequence, it is often used as an acronym for "raising elephants is so utterly boring". What does this sequence accomplishes? Holding alt+sysrq key, we proceed pressing the command keys in sequence, and this is what happens: REISUB

First of all r switches the keyboard from raw to XLATE mode, then, e sends a SIGTERM signal to all processes, so that they can be closed in a graceful way if possibile. After that we send a SIGKILL signal by pressing i, to terminate remaining process which didn't respond to the previous signal. With s we try to sync all mounted filesystems and flush all changes from cache to the disk immediately. By using u we remount all filesystems in read only mode, and finally by pressing b, we perform a system reboot.
