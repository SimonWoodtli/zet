# What Are Linux Security Modules?

A modern computer system must be made secure, but needs vary according to
sensitivity of data, number of users with accounts, exposure to outside
networks, legal requirements and other factors. Responsibility for enabling
good security controls falls both on application designers and the Linux kernel
developers and maintainers. Of course, users have to follow good procedures as
well, but on a well-run system, non-privileged users should have very limited
ability to expose the system to security violations. In this section, we are
concerned with how the Linux kernel enhances security through the use of the
Linux Security Modules framework, particularly with the deployment of SELinux.
The idea is to implement mandatory access controls on requests made to the
kernel, but to do so in a way that:

* Minimizes changes to the kernel
* Minimizes overhead on the kernel
* Permits flexibility and choice between different implementations, each of
  which is presented as a self-contained LSM (Linux Security Module)

The basic idea is to hook system calls; insert code whenever an application
requests a transition to kernel (system) mode in order to accomplish work that
requires enhanced abilities; this code makes sure permissions are valid,
malicious intent is protected against, etc. It does this by invoking
security-related functional steps before and/or after a system call is
fulfilled by the kernel.

Tags:

    #linux #sysadmin #security #LFS207 #LSM
