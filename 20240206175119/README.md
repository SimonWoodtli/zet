# Sign, Encrypt and Decrypt Files with PGP/GPG

> 📝 If your S,E,A private subkeys are on a Yubikey you need to enter the PIN
> once and if you have touch policy enabled touch the Yubikey every time when
> interacting with gpg actions.

## 1. Sign Files

### Method 1: Verifies the new output signature file:

Signature on the same file (readable):

* To sign a file (output readable): `gpg --clear-sign document.txt`
* To just verify the file: `gpg --verify document.txt.asc`
* To verify and get output of file: `gpg --decrypt document.txt.asc`

Signature on the same file but encoded as binary (non-readable):

* To sign a file (output binary): `gpg --sign document.txt`
* To just verify: `gpg --verify document.txt.gpg`
* To verify a file and get the binary back to readable: `gpg --decrypt document.txt.gpg` (to see a half way ok hex output: `xxd document.txt.gpg`)

Signature on same file but encoded as ASCII-Armor (non-readable):

* To sign a file (output ASCII-Armor): `gpg --sign --armor document.txt`
* To just verify: `gpg --verify document.txt.asc`
* To verify a file and get the ASCII-Armor back to readable: `gpg --decrypt document.txt.asc`

> 📝 This is normal, it means verification is performed on the output file .asc or .gpg, but the original file is not getting verified: 'gpg: WARNING: not a detached signature; file 'document.txt' was NOT verified!'

### Method 2: Verifies the original file

Signature on a separate file:

* To sign a file as a detached signature: `gpg --output document.txt.sig --detach-sig document.txt`
* To verify a detached signature: `gpg --verify document.txt.sig document.txt`

Of course you can also use --armor and binary whatever suits your best.

## 2. Encrypt/Decrypt Files

### Encrypt

To encrypt a file: `gpg -r me@simonwoodtli.com --encrypt document.txt`

### Decrypt

To decrypt a file: `gpg --decrypt document.txt.gpg > document.txt`

Related:

* <https://www.hacksanity.com/kb/gnupg-part-3-import-sign-trust-keys/>
* [20240205004612](/20240205004612/) Create a PGP/GPG pairkey
* [20240205234225](/20240205234225/) Add private PGP/GPG subkeys (A,E,S) to your Yubikey

Tags:

    #linux #PGP #GPG #security #sysadmin #development
