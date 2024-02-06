# Create a PGP/GPG pairkey

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
11. Create a revoke certificate (in case your masterkey gets compromized): `gpg --gen-revoke me@simonwoodtli.com > ~/my_keys/revoke.asc`
11. Export your public key: `gpg --armor --export me@simonwoodtli.com ~/my_keys/public.key`
11. Export all three subkeys(private): `gpg --armor --export-secret-subkeys me@simonwoodtli.com > ~/my_keys/sub.key`
11. Copy all your fingerprints into a file: `gpg --fingerprint --fingerprint me@simonwoodtli.com > ~/my_keys/fingerprints.txt`
11. Move my_keys dir: `mv ~/my_keys ~/.gnupg`
11. Create tarball: `tar -czf gpg-datadir.tar.gz .gnupg/`
11. Plugin a brand new USB stick and create a luks encrypted device. Then copy
   that tarball in there, unmount an reinsert the stick to test the passphrase
   on it.

Glossar:

* sbb> means a subkey inside a yubikey cannot be reused/exported anywhere, so
  even if someone steals or has access to your yubikey they cannot export the
  private keys
* sbb# means a subkey that is currently not imported/present in ~.gnupg
* sbb* means you selected this subkey

## Next Steps

TODO: Add all subkeys to your yubikey: add zet
TODO: Use that subkeys on your yubikey to create a `pass` database: add zet
TODO: Use that (A) subkey on your yubikey for SSH authentication: add zet

Related:

* <https://tails.net/>
* <https://musigma.blog/2021/05/09/gpg-ssh-ed25519.html>
* <https://onerng.info/>

Tags:

    #linux #security #infosec #GPG #PGP #yubikey #tails #LUKS
