# Compress files and add Password/Passphrase protection to extract

## Tarball with AES256 from GPG

> 📝 Best if used for Linux/Unix systems because it preserves metadata about files.

* Compress: `tar czvpf - file1.txt file2.pdf file3.jpg | gpg --symmetric --cipher-algo aes256 -o myarchive.tar.gz.gpg`
* Extract: `gpg -d myarchive.tar.gz.gpg | tar xzvf -`

## Zip built-in PKZIP or AES

> 📝 DO NOT USE the zip format for backup purpose on Linux/Unix because:
> zip does not store the owner/group of the file.

### AES256

* Compress(recommended): `zip --encrypt myarchive.zip file1.txt file2.pdf file3.jpg`

### PKZIP or ZIP2.0 (insecure/weak encryption algo)

* Compress(bad): `zip --password PASSPHRASE myarchive.zip file1.txt file2.pdf file3.jpg`
* Compress(better): `zip --password $(pass passphrase/7zip) myarchive.zip file1.txt file2.pdf file3.jpg`
* Extract: `unzip myarchive.zip`

## 7zip built-in AES256

> 📝 DO NOT USE the 7-zip format for backup purpose on Linux/Unix because:
> 7-zip does not store the owner/group of the file.

> ⚠️ If you use -pPASSPHRASE your secret gets exposed to the terminal which is
> bad practice. Best to add an entry into pass 'passphrase/7zip' and use
> -p\$(pass ...) instead. Or omit 'PASSPHRASE' and it will prompt you for a passphrase interactively.

`7z` and `7za` are essentially the same cli. However `7za` is cross-platform
compatible and has less options. Creating passphrases is possible with both
CLIs. I just used one example for .zip and another for .7z but both can be used
for these file types.

### .zip file

* Compress(bad): `7za a -tzip -p'PASSPHRASE' -mem=AES256 secure.zip file1.txt file2.pdf file3.jpg`
* Compress(better): `7za a -tzip -p -mem=AES256 secure.zip file1.txt file2.pdf file3.jpg`
* Compress (recommended): `7za a -tzip -p$(pass passphrase/7zip) -mem=AES256 secure.zip file1.txt file2.pdf file3.jpg`
* Extract: `7za e myarchive.zip`

### .7z file

* Compress(bad): `7z a -t7z -m0=lzma2 -mx=9 -mfb=64 -md=32m -ms=on -mhe=on -p'eat_my_shorts' myarchive.7z dir1`
* Compress(better): `7z a -t7z -m0=lzma2 -mx=9 -mfb=64 -md=32m -ms=on -mhe=on -p myarchive.7z dir1`
* Compress(recommended): `7z a -t7z -m0=lzma2 -mx=9 -mfb=64 -md=32m -ms=on -mhe=on -p$(pass passphrase/7zip) myarchive.7z dir1`
* Extract: `7z e myarchive.7z`

Related:

* <https://www.putorius.net/how-to-create-enrcypted-password.html>

Tags:

    #linux #gpg #7zip #zip #AES #encryption #security #tarball
