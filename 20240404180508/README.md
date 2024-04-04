# systemd features

Instead of bash scripts(old init method), systemd uses .service files. Granular
logging is provided by default using the name of the unit. Tracking and
creating user processes without root privileges is another powerful ability of
systemd.

* Boots faster than previous init systems
* Provides aggressive parallelization capabilities
* Uses socket and D-Bus activation for starting services
* Replaces shell scripts with programs
* Offers on-demand starting of daemons
* Keeps track of processes using cgroups
* Maintains mount and automount points
* Implements an elaborate transactional dependency-based service control logic
* Provides detailed start and stop functionality, including similar abilities to cron

Related:

* [20240404175613](/20240404175613/) systemd init

Tags:

    #linux #sysadmin #systemd #daemon #boot #init #LFS207
