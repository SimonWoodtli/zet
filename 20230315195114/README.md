# age - cheatsheet

## Own Usage with priv/pub key

Since you are alone when encrypting a file you only need one recipient.

* create private/public keypair: `age-keygen -o /tmp/key.txt`
* encrypt file foo: `age -o encrypted.foo.age -r age1w3yy8q2acldnnnns3t9e4us72c0sjkgx9hzaaasskdwtjrvmwelqqyfx03 foo`
* decrypt file foo: `age -d -i /tmp/key.txt -o decryptedfoo encrypted.foo.age`

## Shared Usage with priv/pub key

If you want to share an encrypted file you need to add multiple recipients when you encrypt the file.

Method 1:
* encrypt file foo: `age -o encrypted.foo.age -r <public-id1> -r <public-id2> foo`

Method 2:
* create recipient file: `printf "%s\n" "<public-id1>" "<public-id2> "3...n" " > /tmp/recipient.txt`
* encrypt foo with recipient file: `age -o encrypted.foo.age -R /tmp/recipient.txt foo`

Decrypting will be the same as shown before. Because you need your private key to validate.


## Shared/Alone Usage with passphrase

* encrypt file foo with passphrase: `age -o encrypted.foo.age -p foo`
* decrypt file foo with passphrase: `age -d -o decryptedfoo -o encrypted.foo.age`

## Own Usage with public SSH key

* encrypt file foo with public ssh key: `age -o encrypted.foo.age -R ~/.ssh/id_ed25519.pub foo`
* decrypt file foo with private ssh key: `age -d -i ~/.ssh/id_ed25519 -o decryptedfoo encrypted.foo.age`

## Own Usage with Yubikey

Gotta research
