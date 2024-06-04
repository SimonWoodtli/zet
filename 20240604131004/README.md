# Linux LSM: AppArmor

AppArmor is an LSM alternative to SELinux. Support for it has been incorporated
in the Linux kernel since 2006. It has been used by SUSE, Ubuntu and other
distributions.

AppArmor:

* Provides Mandatory Access Control (MAC)
* Allows administrators to associate a security profile to a program which restricts its capabilities
* Is considered easier (by some but not all) to use than SELinux
* Is considered filesystem-neutral (no security labels required)

AppArmor supplements the traditional UNIX Discretionary Access Control (DAC)
model by providing Mandatory Access Control (MAC).

In addition to manually specifying profiles, AppArmor includes a learning mode,
in which violations of the profile are logged, but not prevented. This log can
then be turned into a profile, based on the program's typical behavior.

Tags:

    #linux #sysadmin #security #LSM #LFS207
