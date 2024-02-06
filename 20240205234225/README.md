# Add private PGP/GPG subkeys (A,E,S) to your Yubikey

Why store your PGP private subkeys on a yubikey?

1. Because all cryptographic operations will happen on the yubikey. That means
   they are not getting exposed to the host machine and are not in your OS memory.
1. Your private keys stored in a yubikey are non exportable (> sign) so even
   with physical access to the yubikey no one can steal your private keys.
1. You don’t have to remember nor type strong passphrases to access private
   keys. If you activate touch feature you simply need to touch the yubikey.
1. The yubikey will brick itself after three failed attempts to unlock them
   with a PIN.

## Prerequisites

* [20240205004612](/20240205004612/) Create a PGP/GPG pairkey

## 1. Prepare Yubikey to use GPG feature on your daily driver

1. Install dependencies for yubikey

```
apt install scdaemon yubikey-manager libpam-yubico libpam-u2f libu2f-udev
dnf install pcsc-tools yubikey-manager pam_yubico pam-u2f pamu2fcfg u2f-hidraw-policy
```

## 2. Add Subkeys on Tails OS

1. Add subkeys to your yubikey on air gapped machine running tails OS.

Related:

* <https://docs.fedoraproject.org/en-US/quick-docs/using-yubikeys/>
* <https://wiki.archlinux.org/title/YubiKey>
