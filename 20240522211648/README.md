# What needs Backup?

Always:

* Business-related data
* System configuration files
* User files (usually under /home)

Maybe:

* Spooling directories (for printing, mail, etc.)
* Logging files (found in /var/log, and elsewhere)

Probably Not:

* Software that can easily be re-installed; on a well-managed system, this should be almost everything
* The /tmp directory, because its contents are indeed supposed to be only temporary
* Pseudo-filesystems such as /proc, /dev and /sys
* Any swap partitions or files

Tags:

    #linux #sysadmin #backup #LFS207
