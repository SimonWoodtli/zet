# Install and Setup `mutt`

TODO: Gotta rewrite this

## Alpine Linux

### PAM-GnuPG

1. (With Distrobox this is not needed) `adduser -h /var/home/xnasero -s /bin/bash -S -D xnasero`
1. `sudo apk add automake autoconf libtool build-base linux-pam-dev`
1. `git clone https://github.com/cruegge/pam-gnupg.git`
1. `sudo apk add gnupg` (you can have gpg on the host and in the container too)
    1. I had to run `gpg --full-generate-key` on the host, in the container it throws permission error. But later you can still access the key with gpg in the container.

```
./autogen.sh
./configure
sudo make
sudo make install
```

5. `pass init <gpgEmail>` or `pass init <gpgID>`
6. `sudo vi /etc/pam.d/system-local-login` and add:

```
auth     optional  pam_gnupg.so store-only
session  optional  pam_gnupg.so
```

7. `gpg -K --with-keygrip` copy paste the keygrip from the keypair you associated with `pass`.
8. `echo <yourCopiedKEYGRIP> > $HOME/.config/pam-gnupg` (only the 40 digits are needed)


### Mutt-Wizzard

1. `sudo apk add notmuch abook cronie ca-certificates pass msmtp isync neomutt chezmoi`
1. `git clone https://github.com/LukeSmithxyz/mutt-wizard.git`


## Instructions

1. install neomutt with the `install-mutt` script
1. create gpg key: `gpg --full-gen-key` -> 
1. Create a new entry with the email/passphrase in your password manager
1. Unlock gpg at startup so you don't have to enter passphrase every time you use gpg.
  1.On Gnome you can safe the passphrase by checking the box when you use your gpg key or when you setup a new gpg key.
  However if you use a window manager you need to setup `pam-gnupg`.
  2. You can trigger to enter your passphrase `touch foo && gpg -r [gpgEmail] -e foo && gpg -d foo.gpg` and check box to store the passphrase on the Gnome keyring.
2. Tell `pass` the local password manager, which gpg key to use: `pass init [gpgEmail]` or `pass init [gpgID]`. This allows neomutt to store the emails passwords in `pass` and use `gpg` to encrypt/decrypt these passwords. 
Note: `pass` stores entries in ~/.password-store/
4. add your emails with `mw -a yourEmail`
5. setup cronjob to sync mail every 10min: `mw -t 10` check:  `crontab -l`

(New Mutt-Wizzard should not need this): setup `notmuch` to search mails with ctrl-f: `notmuch new`

## Issue mailto from webbrowser

If you set xdg-open to open neomutt you need to invoke it with `alacritty -e /usr/bin/neomutt -E %U` in a neomutt.desktop file.However invoking alacritty like this will actually open a login shell which is not able to parse my bashrc early enough. That means no environment variables are set. The thing is that in ssh sessions bashrc gets loaded (I guess because I already started an interactive non-login shell). And if I open a login shell with `CTRL-ALT-F3` bashrc gets loaded as well.
So that is why I think it actually works like it should but bashrc just doesn't get loaded quick enough. A keyboard shortcut to test this: `alacritty --hold -e env` which shows only a bare minimum of the enviroment is being loaded.

TODO: Figure out how to read bashrc before login shell even starts.
* Simply reading bashrc in .profile or .bash_profile won't suffice: `[ -f "$HOME/.bashrc" ] && source "$HOME/.bashrc"`

The consequence of this is that nano gets started in this instance. A quick fix is to either:
1. add this to muttrc: `set editor="/usr/bin/vim"`
2. or add this to .bash_profile: `export EDITOR=vim` (preferred)

[mutt-wizzard]: <https://github.com/SimonWoodtli/dotfiles/blob/main/bin/install/popOS/install-mutt>

Related:

* <https://github.com/LukeSmithxyz/mutt-wizard>

Tags:

    #linux #mutt #email
