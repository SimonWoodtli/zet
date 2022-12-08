# Remove password access to server via ssh

> 🧐 Before you remove password access make sure you added your clients public 
sshkey to ~/.ssh/authorized_keys on the server. You can do this manually or use `ssh-copy-id`.

Make sure to use sshd_config and not ssh_config

1. `sudo vi /etc/ssh/sshd_config`
1. go to `#PasswordAuthentication yes` change to `PasswordAuthentication no`
1. optional if you dont want root access: `PermitRootLogin no`

Tags:

    #linux #ssh #security #server
