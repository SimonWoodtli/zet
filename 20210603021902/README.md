# How to give a normal user sudo access?

If your system does not already have sudo set up and enabled, you need to do the following steps:

You will need to make modifications as the administrative or superuser, root. While sudo will become the preferred method of doing this, we do not have it set up yet, so we will use su (which we will discuss later in detail) instead. At the command line prompt, type su and press Enter. You will then be prompted for the root password, so enter it and press Enter. You will notice that nothing is printed; this is so others cannot see the password on the screen. You should end up with a different looking prompt, often ending with ‘#’. For example: `$ su`

Now, you need to create a configuration file to enable your user account to use sudo. Typically, this file is created in the /etc/sudoers.d/ directory with the name of the file the same as your username. For example, for this demo, let’s say your username is student. After doing step 1, you would then create the configuration file for student by doing this:  
`echo "student ALL=(ALL) ALL" > /etc/sudoers.d/student`

Finally, some Linux distributions will complain if you do not also change permissions on the file by doing:
`chmod 440 /etc/sudoers.d/student`

Tags:
    
    #linux #sysadmin #terminal #userManagement
