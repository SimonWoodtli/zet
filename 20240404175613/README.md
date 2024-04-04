# systemd init

> 📝 Three most common implementations include systemd, Upstart and SysVinit,
> but all major distributions have now moved to systemd.

The /sbin/init program (usually just called init) is the first user-level
process (or task) run on the system and continues to run until the system is
shut down. Traditionally, it has been considered the parent of all user
processes, although technically that is not true, as some processes are started
directly by the kernel.

init coordinates the later stages of the boot process, configures all aspects
of the environment, and starts the processes needed for logging into the
system. init also works closely with the kernel in cleaning up after processes
when they terminate.

## History: SysVinit

> 📝 Migration to systemd was non-trivial and bugs and missing features could
> be very disabling, so essential compatibility layers were adopted and still
> exist for legacy software. Thus, SysVinit utility compatibility wrappers
> still persist.

Traditionally, nearly all distributions based the init process on UNIX's
venerable SysVinit' software. However, this scheme was developed decades ago
under rather different circumstances:

* The target was multi-user mainframe systems (and not personal computers, laptops, and other devices)
* The target was a single processor system
* Startup (and shutdown) time was not an important matter; it was far less important than getting things right.

Startup was viewed as a serial process, divided into a series of sequential
stages (termed run levels). Each stage required completion before the next
could proceed. Thus, startup did not easily take advantage of the parallel
processing that could be done on multiple processors or cores.

Secondly, shutdown/reboot was seen as a relatively rare event, and exactly how
long it took was not considered important; today Linux systems usually boot in
a manner of seconds.

## History: Upstart

* Developed by Ubuntu and first included in 2006
* Adopted in Fedora 9 (in 2008) and in RHEL 6 and its clones
* Was also used in various embedded and mobile devices

Every other major Linux distribution has now adopted systemd and configured it
as the default. Even Ubuntu phased out Upstart in its favor.

Related:

* [20240404180508](/20240404180508/) systemd features

Tags:

    #linux #sysadmin #systemd #init #boot #LFS207
