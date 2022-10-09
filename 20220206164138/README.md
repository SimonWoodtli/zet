# Sysadmin: How to create secure passwords

## Password

IT professionals follow several good practices for securing the data and the password of every user.

Password aging is a method to ensure that users get prompts that remind them to create a new password after a specific period. This can ensure that passwords, if cracked, will only be usable for a limited amount of time. This feature is implemented using chage, which configures the password expiry information for a user.
Another method is to force users to set strong passwords using Pluggable Authentication Modules (PAM). PAM can be configured to automatically verify that a password created or modified using the passwd utility is sufficiently strong. PAM configuration is implemented using a library called pam_cracklib.so, which can also be replaced by pam_passwdqc.so to take advantage of more options.
One can also install password cracking programs, such as John The Ripper, to secure the password file and detect weak password entries. It is recommended that written authorization be obtained before installing such tools on any system that you do not own.

## Exercise

With the newly created user from the previous exercise, look at the password aging for the user.
Modify the expiration date for the user, setting it to be something that has passed, and check to see what has changed.
When you are finished and wish to delete the newly created account, use userdel, as in:

`sudo userdel newuser`

## Solution

```sh
chage –list newuser
sudo chage -E 2014-31-12 newuser
chage –list newuser
sudo userdel newuser
```

> Note: The example solution works in the US. `$ chage -E` uses the default date format for the local keyboard setting.

Tags:

    #sysadmin #linux #password #security
