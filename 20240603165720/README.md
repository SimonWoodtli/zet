# Main LSM Choices

For a long time, the only enhanced security model implemented was SELinux. When
the project was first floated upstream in 2001 to be included directly in the
kernel, there were objections about using only one approach to enhanced
security.

As a result, the LSM approach was adopted, where alternative modules to SELinux
could be used as they were developed and was incorporated into the Linux kernel
in 2003.

Originally, only one LSM could be used at a time as they can potentially modify
the same parts of the Linux kernel. However, it is now possible to stack them
with some care. LSMs are now considered as either major or minor when
configuring their combination.

The current LSM implementations are:

* SELinux
* AppArmor
* Smack
* Tomoyo
* Yama

We will concentrate primarily on SELinux and secondarily on AppArmor in order of usage volume.

See [A Brief Tour of Linux Security Modules][lsm] by Shaun Ruffell for a nice review of the mechanism and choices.

[lsm]:<https://www.starlab.io/blog/a-brief-tour-of-linux-security-modules>

Tags:

    #linux #sysadmin #security #LSM #LFS207
