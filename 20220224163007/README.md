# Remove password access to server via ssh

Make sure to use sshd_config and not ssh_config

1. `sudo vi /etc/ssh/sshd_config`
1. go to `#PasswordAuthentication yes` change to `PasswordAuthentication no`

Tags:

    #linux #ssh #security #server
