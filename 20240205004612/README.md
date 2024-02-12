# Create a PGP/GPG pairkey

## Prerequisites

* 2x Brand new USB Stick (1 tails, 1 LUKS backup)
* Brand new SD Card to export public.key and fingerprints
* Tails installed on brand new USB stick
* Air gapped machine

## Setup

> ⚠️ Everytime you want to interact with the masterkey you need to boot up a
> live session of tails on an airgapped device, insert your LUKS encrypted USB
> and create new subkeys or whatever you need to do.

Why create a masterkey certify(C) and three separate subkeys
authentication(A), signing(S), encryption(E)?

Because that means you will have one main fingerprint and you will keep that
fingerprint. Even if your subkeys expire you can create new subkeys from your
masterkey anytime. This gives you the advantage of having a non expiring
masterkey with expiring subkeys. And separating the keys to a single core
feature signing, encrypting or authenticating is just another best practice.

RSA vs ECC(with ED25519)?

In short if your yubikey firmware is >= 5.2.3 it should support ed25519 and you
should create a masterkey with ed25519. If your yubikey is a bit older go for
RSA.

1. Run a live image of tails OS on a air gapped device (rip out wifi/bluetooth
   card, any HDDs, ethernet card, best old laptop with no ethernet port)
1. Verify image and put it on Ventoy Stick, or flash it on a separate new USB
   Stick if you feel safer that way :)

```
gpg --import tails-signing.key
gpg --verify tails-amd64-5.22.img.sig tails-amd64-5.22.img
```

> 🧐 To export less sensitive data from tails OS use `qr < foo.txt`

3. Bootup tails OS
3. Remove pgp dir: `rm -r ~/.gnupg`
3. Create dir: `mkdir ~/my_keys`
3. Create a passphrase file for the masterkey (C) `openssl rand -base64 32 > ~/my_keys/passphrase.txt`
3. Copy passphrase to clipboard
3. Create a 'Certify' masterkey (C):

> 🧐 If your yubikey supports it use ed25519 for your masterkey otherwise rsa4096

```
gpg --quick-generate-key \
    'Simon D. Woodtli <me@simonwoodtli.com>' \
    ed25519 cert never
```

> 🧐 Name/email: If for work/business use real name, known email. If Personal/private use alias for name and unknown email. Look into PGP web of trust.

9. Copy the fingerprint `gpg -K` and export it: `export KEYFP=....`
9. Create three subkeys for E,A,S: (E needs to be cv25519, because ed25519 does
   only C,S,A). (adjust this if you use RSA masterkey)

```
gpg --quick-add-key $KEYFP ed25519 sign 3y
gpg --quick-add-key $KEYFP cv25519 encr 3y
gpg --quick-add-key $KEYFP ed25519 auth 3y
```

11. Export the masterkey(private): `gpg --armor --export-secret-keys me@simonwoodtli.com > ~/my_keys/master.key`
      * This stays on your encrypted USB Stick never export it anywhere else!
11. Create a revoke certificate (in case your masterkey gets compromized): `gpg --gen-revoke me@simonwoodtli.com > ~/my_keys/revoke.asc`
      * This stays on your encrypted USB Stick never export it anywhere else!
11. Export all three subkeys(private): `gpg --armor --export-secret-subkeys me@simonwoodtli.com > ~/my_keys/sub.key`
      * This stays on your encrypted USB Stick never export it anywhere else!
11. Export your public key: `gpg --armor --export me@simonwoodtli.com ~/my_keys/public.key`
      * Plugin your SD Card, format it and copy the public.key to the SD Card
      * Tried to use `qr` and image to text features both can't render such a long key
11. Copy all your fingerprints into a file: `gpg --fingerprint --fingerprint me@simonwoodtli.com > ~/my_keys/fingerprints.txt`
      * Copy the fingerprints.txt to the SD Card
11. Move my_keys dir: `mv ~/my_keys ~/.gnupg`
11. Create tarball: `tar -czf gpg-datadir.tar.gz .gnupg/`
11. Plugin a brand new USB stick and create a luks encrypted device. Then copy
   that tarball in there, unmount an reinsert the stick to test the passphrase
   on it.
11. Test the tarball: `rm ~/.gnupg/` and copy the tarball from the USB stick to ~. Then extract it and see if `gpg --list-secret-keys` works and if all files are there. (my_keys/ is important)
11. Unplug Both SD Card and USB stick
11. Plugin your SD Card into your daily driver and save both public.key and
    fingerprints.txt in your password manager. Also copy them on your local
    machine in a folder of your choice.

Useful info about GPG agent:

* sbb> means a subkey that is on an external device (not on your local machine,
  but yubikey). Also it cannot be reused/exported anywhere. So even if someone
  steals or has access to your yubikey they cannot export the private keys
* sbb# means a subkey that is currently not imported/present meaning it's missing
* sbb* means you selected this subkey

## Next Steps

* [20240205234225](/20240205234225/) Add private PGP/GPG subkeys (A,E,S) to your Yubikey
* [20240206140818](/20240206140818/) Add `pass` with Yubikey burned in PGP keys

TODO: Use that (A) subkey on your yubikey for SSH authentication: add zet

Related:

* [20240206175119](/20240206175119/) Sign, Encrypt and Decrypt Files with PGP/GPG
* <https://tails.net/>
* <https://musigma.blog/2021/05/09/gpg-ssh-ed25519.html>
* <https://onerng.info/>
* <https://www.hacksanity.com/kb/gnupg-create-manage-keys/>
* <https://gock.net/blog/2020/gpg-cheat-sheet>
* <https://wiki.debian.org/Subkeys>
* <https://wiki.debian.org/OfflineMasterKey>
* <https://wiki.debian.org/GnuPG/AirgappedMasterKey>
* <https://en.wikipedia.org/wiki/Web_of_trust>

Tags:

    #linux #security #infosec #GPG #PGP #yubikey #tails #LUKS
