# Backup Strategies

We should note that backup methods are useless without associated restore
methods. You have to take into account the robustness, clarity and ease of both
directions when selecting strategies.

The simplest backup scheme is to do a full backup of everything once, and then
perform incremental backups of everything that subsequently changes. While full
backups can take a lot of time, restoring from incremental backups can be more
difficult and time consuming. Thus, you can use a mix of both to optimize time
and effort.

An example of one useful strategy involving tapes (you can easily substitute
other media in the description):

1. Use tape 1 for a full backup on Friday
1. Use tapes 2-5 for incremental backups on Monday-Thursday
1. Use tape 6 for full backup on second Friday
1. Use tapes 2-5 for incremental backups on second Monday-Thursday
1. Do not overwrite tape 1 until completion of full backup on tape 6
1. After full backup to tape 6, move tape 1 to external location for disaster recovery
1. For next full backup (next Friday) exchange tape 1 for tape 6

A good rule of thumb is to have at least two weeks of backups available.

Tags:

    #linux #sysadmin #backup #data #LFS207
