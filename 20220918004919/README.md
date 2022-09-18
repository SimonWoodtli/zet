# Fix Gnome-Keyring Issue

So this already happened a couple of times. A program or task I did (don't know what) caused to change the gnome-keyring. After that the gnome-keyring prompts a message it needs that the current user password no longer matches with the  gnome keyring.

## Delete old keyring and create new one

Caused cause of a bug changed your password, or you forgot your password. Of course all your stored passwords will be wiped with this. Unfortunately this is the only thing you can do in this case.

1. rm the current keyring (two files, login and user: `rm ~/.local/share/keyrings/login.keyring`
2. create new keyring: run `seahorse` and use GUI

## Update password on your keyring

If you still have and remember the old password but changed the system. You can just install `seahorse` and update the password to the new users password.
