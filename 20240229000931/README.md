# Use Yubikey to authenticate SSH via burned in PGP/GPG subkeys

Currently it's not working on my machine with Fedora Silverblue. I will have a look if it works again when F40 is released.

> 🧐 If on f silvervlue remove opensc: `rpm-ostree override remove opensc`
> 🧐 Logs for Authentication: `tail -f /var/log/auth.log` or `journalctl | grep ssh`

1. add to bashrc:

```
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
export GPG_TTY=$(tty)
```

2. copy Auth subkey fingerprint: `gpg --card-status`
2. create ssh key from gpg auth subkey: `gpg --export-ssh-key 6E8130C00B556DD5532CC8FB767FE4C5649614D9 > ~/.ssh/$(isosec).pub`
3. test if it works


Optionally you may need to add some gpg configurations:

1. add to .gnupg/gpg-agent.conf

```
enable-ssh-support
use-standard-socket
```

1. add to .gnupg/scdaemon.conf

```
disable-ccid
reader-port Yubico YubiKey
```

1. add to .gnupg/sshcontrol: your [A] keygrip `gpg --list-key --with-keygrip`

```
A1FBB7D85BCC9A0441D42754AC8DFE822781E11F
```

Related:

* <https://wiki.archlinux.org/title/GnuPG#SSH_agent>
* <https://bugzilla.redhat.com/show_bug.cgi?id=1893131>
* <https://discussion.fedoraproject.org/t/using-yubikey-for-ssh-always-asking-for-password-gnome/75038?replies_to_post_number=18>
* <https://developer.okta.com/blog/2021/07/07/developers-guide-to-gpg#enable-your-gpg-key-for-ssh>
