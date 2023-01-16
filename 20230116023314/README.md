# Difference between `su` and `su -`

> 🧐 If no username is given it will switch you to the root user, but you
probably need to prefix sudo to get permission.

Both `su` and `su -` can be used to switch users from the command line, but the
latter starts up a new login shell. So the difference is `su foo` switches your 
current user to foo, but it won't load foo's environment. That means you will
still have the previous users environment. Where as `su - foo` will actually
load foo's envrionment meaning it will source the new .bashrc .profile etc.

Related:

* <https://www.tecmint.com/difference-between-su-and-su-commands-in-linux/>
* <https://tecadmin.net/difference-between-login-and-non-login-shell/>
