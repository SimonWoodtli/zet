# Backup Methods

You should never have all backups residing in the same physical location as the
systems being protected. Otherwise, fire or other physical damage could lead to
a total loss. In the past, this usually meant physically transporting magnetic
tapes to a secure location. Today, this is more likely to mean transferring
backup files over the Internet to alternative physical locations. Obviously,
this has to be done in a secure way, using encryption and other security
precautions as is appropriate.

Several different kinds of backup methods can be used, often in concert with
each other.

Full:  
Backup for all files on the system.

Incremental:  
Backup for all files that have changed since the last incremental or full
backup.

Differential:  
Backup for all files that have changed since the last full backup.

Multiple level incremental:  
Backup for all files that have changed since the previous backup at the same or a previous level.

User:  
Backup only for files in a specific user's directory.

Tags:

    #linux #sysadmin #backup #data #LFS207
