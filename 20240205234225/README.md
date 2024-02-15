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

## 1. Prepare Yubikey to use PGP keys for your daily driver

1. Startup your regular machine
1. If you already used gnupg before create a tarball backup of the old keys and
   store them somewhere save (you already might have compromised them since. They
   weren't created using an air gapped device and the private keys are stored
   on the host machine and not a Yubikey, and your PGP passphrase stored in Password Manager).
      1. Once backup of old keys is done rm dir: `rm -r ~/.gnupg/`
      1. Import the public key: `gpg --import ~/Private/bin/pgp/public.key` (or wherever you copied it from the SD card)
      1. Trust the key ultimately: `gpg --edit-key me@simonwoodtli.com`
      1. Check `gpg --list-keys`
      1. Notice `gpg --list-secret-keys` should be empty. However once your
         yubikey has the subkeys on it and gets plugged in, it will list the
         secret keys with a '>' sign (which means key on external device)
1. Install dependencies for yubikey:

```
apt install scdaemon yubikey-manager libpam-yubico libpam-u2f libu2f-udev
dnf install pcsc-tools yubikey-manager pam_yubico pam-u2f pamu2fcfg
```

4. Check if gpg card daemon works: `gpg --card-status` (If no actual data shows up try systemctl/gpg agent restart)
      * maybe `sudo systemctl restart pcscd`  or `sudo systemctl enable pcscd --now`
      * kill gpg agent: `gpgconf --kill gpg-agent` and restart: `gpg-connect-agent updatestartuptty /bye`
4. Get info about openpgp (touch policies are off by default): `ykman openpgp info`
4. Enable Touch Policies for S,A,E when using GPG via Yubikey (touching yubikey required)

```
ykman openpgp keys set-touch -h
ykman openpgp keys set-touch enc on
ykman openpgp keys set-touch sig on
ykman openpgp keys set-touch aut on
```

7. Check if enabled: `ykman openpgp info`

## 2. Add PGP Subkeys on Tails OS

> 🧐 To export less sensitive data from tails OS use `qr < foo.txt`

> 📝 Default Admin-PIN: 12345678, Default PIN: 123456
> 📝 If you forget/lose your PINs you have to factory reset your Yubikey. Either do a full wipe or only pgp app.

> ⚠️ `gpg --card-edit` then 'admin' and 'factory-reset' will wipe the entire
> Yubikey! Including OATH, OTP and the rest of the data. If you only want to
> delete the gpg related data only, use `ykman openpgp reset`. See yubico support article

1. Startup Tails OS on an air gapped device
1. Change regular PIN and Admin-PIN: `gpg --card-edit`

> 🧐 The regular PIN is used when encrypting data with the Yubikey (on touch).
> It can be a weak PIN because after 3 failed attempts the Yubikey is locked.
> The admin PIN should be a strong passphrase, don't just use numbers
> alphanumeric values are allowed (min. 200 entropy).

```
admin
passwd
```

> ⚠️m Never touch the original tarball on your USB stick. Just copy it to your ~.

3. Remove current gnupg dir: `rm ~/.gnupg`
3. Mount your encrypted USB stick with the GPG data tarball and copy the tarball into ~ then unzip the copied tarball.
3. Check if all keys are present: `gpg --list-secret-keys`
3. Cat the passphrase.txt and admin PIN to another terminal window. So you can copy them when needed.
3. Add subkeys to your yubikey: `gpg --edit-key me@simonwoodtli.com`

> 🧐 Use the 'key' subcommand to select a key (if it has sbb* it means it's
> selected) and then 'keytocard' to burn it into the yubikey

```
help
key 1
keytocard
key 1
key 2
keytocard
key 2
key 3
keytocard
save
```

8. Check: `gpg --card-status` should list your PGP keys
8. Shutdown Tails, stick Yubikey in your daily driver and check `gpg --card-status` and `ykman openpgp info`

Related:

* <https://docs.fedoraproject.org/en-US/quick-docs/using-yubikeys/>
* <https://wiki.archlinux.org/title/YubiKey>
* <https://support.yubico.com/hc/en-us/articles/360013761339-Resetting-the-OpenPGP-Application-on-the-YubiKey>
* [20240205004612](/20240205004612/) Create a PGP/GPG pairkey
* [20240206140818](/20240206140818/) Add `pass` with Yubikey burned in PGP keys
* [20240206175119](/20240206175119/) Sign, Encrypt and Decrypt Files with PGP/GPG

Tags:

      #linux #security #infosec #yubikey #PGP #GPG #privacy #encryption
