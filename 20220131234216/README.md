# Linux - activate sysrq key

1. $ `cat /proc/sys/kernel/sysrq`  
If terminal returns anything else but 1 sysrq is not active
1. $ `sudo echo "kernel.sysrq = 1" >> /etc/sysctl.d/99-sysctl.conf`
1. reboot and `cat` again to confirm

More [Information]

[Information]: <https://linuxconfig.org/how-to-enable-all-sysrq-functions-on-linux>
