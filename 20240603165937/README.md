# SELinux Overview

SELinux was originally developed by the United States NSA (National Security
Administration) and has been integral to RHEL for a very long time, which has
brought it a large usage base.

Operationally, SELinux is a set of security rules that are used to determine
which processes can access which files, directories, ports, and other items on
the system.

It works with three conceptual quantities:

1. Contexts
These are labels to files, processes, and ports. Examples of contexts are
SELinux user, role and type.

2. Rules
They describe access control in terms of contexts, processes, files, ports,
users, etc.

3. Policies
They are a set of rules that describe what system-wide access control decisions
should be made by SELinux.

A SELinux context is a name used by a rule to define how users, processes,
files and ports interact with each other. As the default policy is to deny any
access, rules are used to describe allowed actions on the system.

Additional [online documentation][docs] can be found in the SELinux User's and Administrator's Guide.

[docs]:<https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/index>

Tags:

    #linux #sysadmin #security #LSM #LFS207
