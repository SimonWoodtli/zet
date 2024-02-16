# Add `pass` with Yubikey burned in PGP keys

## Prerequisites

* [20240205004612](/20240205004612/) Create a PGP/GPG pairkey
* [20240205234225](/20240205234225/) Add private PGP/GPG subkeys (A,E,S) to your Yubikey

## Setup

1. Import your public key into your GPG keyring on the machine you want to setup pass: `gpg --import public.key`
1. Plugin your Yubikey which has the PGP private keys stored in it
1. Check if GPG agent can talk to Yubikey: `gpg --card-status`
1. Copy the fingerprint of you primary/masterkey into your clipboard: `gpg --list-keys`
1. Initialize a new password storage/database: `pass init yourFingerPrint`

Related:

* TODO add fzf script wrapper URL here, (to make a custom way to handle passwords)
* <https://devhints.io/pass>
* <https://github.com/tadfisher/pass-otp>

Tags:

    #linux #passwordManager #setup #yubikey #PGP #GPG #security #privacy
