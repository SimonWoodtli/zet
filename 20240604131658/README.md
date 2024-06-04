# AppArmor cheatsheet

Distributions that come with AppArmor tend to enable it and load it by default.
Note that the Linux kernel has to have it turned on as well, and, in most
cases, only one LSM can run at a time.

Assuming you have the AppArmor kernel module available, on a systemd-equipped
system you can run this command: `sudo systemctl [start|stop|restart|status]
apparmor`

## Profiles

Linux distributions come with pre-packaged profiles, typically installed either
when a given package is installed, or with an AppArmor package, such as
apparmor-profiles. These profiles are stored in /etc/apparmor.d.

Processes can be run in either of the two modes:

* Enforce Mode: Applications are prevented from acting in ways which are
  restricted. Attempted violations are reported to the system logging files.
  This is the default mode. A profile can be set to this mode with aa-enforce.
* Complain Mode: Policies are not enforced, but attempted policy violations are
  reported. This is also called the learning mode. A profile can be set to this
  mode with aa-complain

## Basic commands

* `aa-status`
* `getcap file` (if return is empty no profile has been assigned to the file)
* `setcap ... file`

## More Utilities

There is a bunch of them usually named /usr/sbin/aa-*  
To get a full ist: `rpm -qil apparmor-utils | grep bin`

Some other tools:

* `complain`
* `decode`
* `disable`
* `enable`

Some commonly used tools:

* Show status of all profiles and processes with profiles: `apparmor_status`
* Show a summary for AppArmor log messages: `apparmor_notify`
* Set a specified profile to complain mode: `complain`
* Set a specified profile to enforce mode: `enforce`
* Unload a specified profile from the current kernel and prevent from being
  loaded on system startup `disable`
* Scan log files, and, if AppArmor events that are not covered by existing
  profiles have been recorded, suggest how to take into account, and, if
  approved, modify and reload `logprof`
* Help set up a basic AppArmor profile for a program: `easyprof`
* check status: `sudo apparmor_status`

Tags:

    #linux #sysadmin #security #LSM #LFS207 #cheatsheet
