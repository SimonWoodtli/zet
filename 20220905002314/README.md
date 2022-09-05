# Install Samba on Fedora, Connect on iOS/Win

Samba is a implementation of the SMB protocol. It is used to share files from a Linux host with Windows or Mac.

## Install and enable Samba

```
sudo dnf install samba
sudo systemctl enable smb --now
firewall-cmd --get-active-zones
sudo firewall-cmd --permanent --zone=FedoraWorkstation --add-service=samba
sudo firewall-cmd --reload
```

## Share a directory inside \$HOME

* Create a username in Samba same to the one you create on your machine: `sudo smbpasswd -a foo`
* Create a directory to be the share for foo, and set the correct SELinux context: 

```
mkdir /home/foo/Samba
sudo semanage fcontext --add --type "samba_share_t" "/home/foo/Samba(/.*)?"
sudo restorecon -R ~/Samba
```

* add this to /etc/samba/smb.conf:

```
[share]
        comment = My Share
        path = /home/foo/Samba
        writeable = yes
        browseable = yes
        public = yes
        create mask = 0644
        directory mask = 0755
        write list = user
```

## Connect on iOS

Go to files and on the triple dots click connect server: => enter the internal IP (LAN) from you Linux machine e.g. `192.168.1.201`

## Connect on Win11

File Explorer => right click Network => Show more options => Map network drive `\\192.168.1.201`
