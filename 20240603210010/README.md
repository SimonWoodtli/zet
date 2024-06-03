# SELinux Context Inheritance

Newly created files inherit the context from their parent directory, but when
moving or copying files, it is the context of the source directory which may be
preserved, which can cause problems.

Continuing the previous example, we see the context of tmpfile was not changed
by moving the file from /tmp to /home/jimih (commands and outputs below):

```
cd /tmp/
touch tmpfile
ls -Z tmpfile
```

Output:

```
-rw-rw-r--. jimih jimih unconfined_u:object_r:user_tmp_t:s0 tmpfile
```


```
cd
touch homefile
ls -Z homefile
```

Output:

```
-rw-rw-r--. jimih jimih unconfined_u:object_r:user_home_t:s0 homefile
```

```
mv /tmp/tmpfile .
ls -Z
```

Output:

```
-rw-rw-r--. jimih jimih unconfined_u:object_r:user_home_t:s0 homefile
-rw-rw-r--. jimih jimih unconfined_u:object_r:user_tmp_t:s0 tmpfile
```

The classical example in which moving files creates a SELinux issue is moving
files to the DocumentRoot directory of the httpd server. On SELinux-enabled
systems, the web server can only access files with the correct context labels.
Creating a file in the /tmp directory, and then moving it to the DocumentRoot
directory, will make the file inaccessible to the httpd server until the
SELinux context of the file is adjusted.

Tags:

    #linux #sysadmin #security #LSM #LFS207
