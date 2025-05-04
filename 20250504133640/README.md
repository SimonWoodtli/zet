# ZFS: Raids explained

* Note that ZFS uses ARC and requires memory, generally you want 1GB of RAM for each TB of storage.
* Have at least 32GB RAM on your system if you want to use ZFS

### Raid0 (Striping)

* Writes data across multiple disks simultaneously
* Maximum performance but no redundancy
* Best suited for temporary storage or scratch spaces
* Min. 1 Disk
* Fault Tolerance: None

### Raid1 (Mirroring)

* Creates exact copies of data across all disks
* Simplest redundancy option
* Excellent read performance (can read from either disk)
* Min. 2 Disk
* Fault Tolerance: 1 Disk (can fail and be recovered)

### Raid10 (Striped Mirrors)

* Combines Raid1 and Raid0 features
* Balanced performance and redundancy
* Can survive multiple drive failures if they're in different mirror pairs
* Min. 4 Disk
* Fault Tolerance: half of your disk stack

### RaidZ1//RaidZ (Single Parity)

* Similar to traditional RAID5
* Calculates parity information across all disks
* Good balance of capacity and protection
* Min. 3 Disk
* Fault Tolerance: 1 Disk

### RaidZ2 (Double Parity)

* Similar to traditional RAID6
* Maintains two separate parity stripes
* Can survive failure of two drives simultaneously
* Min. 4 Disk
* Fault Tolerance: 2 Disks

### RaidZ3 (Triple Parity)

* Most redundant option
* Can survive failure of three drives
* Highest overhead in terms of storage space
* Min. 5 Disk
* Fault Tolerance: 3 Disks

## Important Considerations

### Disk Size Matching

* All disks in a vdev (RAID group) should be identical size
* Mixing different sized disks wastes capacity on larger drives
* Consider future expansion when selecting disk sizes

### Performance Characteristics

* Read performance increases with more disks in Raid0/Z1
* Write performance remains consistent across all RAID levels
* Higher RAID levels (Z2/Z3) have more complex write operations

### Recovery Time

* Raid1 offers fastest recovery from single disk failure
* More complex RAID levels take longer to resilver
* Resilvering time depends on array size and system load

## Practical Recommendations

* For critical data: RaidZ2 or Raid10
* For large storage pools: RaidZ1
* For maximum performance: Raid0 (only if data loss is acceptable)
* For simplicity: Raid1
* For maximum redundancy: RaidZ3

Related:

* [20220903203700](/20220903203700/) Proxmox Install Guide
* <https://www.raidisnotabackup.com/>

Tags:

    #linux #hypervisor #NAS #proxmox #ZFS #filesystem #raid
