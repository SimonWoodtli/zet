# Proxmox: Create LXC Templates and or simply change Container/VM ID

Best way is apparently to create a backup first and then restore it with a new id.

So if you have a LXC with ID 100 and want to make a newer template version.
Don't just convert it directly otherwise the template will have ID 100. 
The better approach:

1. Create Backup of LXC 100: `vzdump 100 --stdout | gzip > /tmp/container-100-backup.tar.gz`
1. Delete the old 200 template: `pct destroy 200`
1. Restore Container with new ID 200: `pct restore 200 /tmp/container-100-backup.tar.gz --storage local-lvm`
1. Convert into template: `pct template 100`
1. Delete old 100 Container: `pct destroy 100`
1. Clone new 200 template: `pct clone 200 100 --storage local-lvm`
  * Syntax: `pct clone <template-id> <new-container-id> --full 4--storage <storage-name>`

Related:

* [20220903203700](/20220903203700/) Proxmox Install Guide
* [20250504133640](/20250504133640/) ZFS: Raids explained
* [20250504201808](/20250504201808/) Install and Setup TrueNAS via Proxmox VM
* [20250505201544](/20250505201544/) Proxmox: Create LXC Templates and or simply change Container/VM ID
