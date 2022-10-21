## Create a RAID
This page aims to give tricks regarding the analysis and recovery of RAID array if faulty.
### Training with MDADM
Use [`dd`](https://www.gnu.org/software/coreutils/) to create three 32MB files:

```
dd if=/dev/zero of=disk1 bs=1M count=32
dd if=/dev/zero of=disk2 bs=1M count=32
dd if=/dev/zero of=disk3 bs=1M count=32
```

Next, map them to loopback block devices using `losetup`:

```
losetup --show -f disk0
losetup --show -f disk1
losetup --show -f disk2
```

There are now three virtual block devices at:

```
/dev/loop0
/dev/loop1
/dev/loop2
```

These can be treated like any other block storage device e.g. given a file system and added into a RAID array.

You can remove the devices when you’re done using the following command for each device:

```
losetup -d /dev/loop0
```

#### Create a RAID array[](https://bash-prompt.net/guides/bash-mdadm-practice/#create-a-raid-array)

Use the `mdadm` command to create a RAID 5 array using the the `/dev/loop` devices:

```
# mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/loop0 /dev/loop1 /dev/loop2
mdadm: layout defaults to left-symmetric
mdadm: layout defaults to left-symmetric
mdadm: chunk size defaults to 512K
mdadm: size set to 30720K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

The options here mean as follows:

-   `--create` - Create a new RAID array.
-   `--verbose`- Print information about what `mdadm` does.
-   `/dev/md0` - The name and location of the array.
-   `--level=5`- Create a RAID 5 array.
-   `--raid-devices=3` - The number of devices in the array.
-   `/dev/loop0 /dev/loop1 /dev/loop2` - The name and path of the devices.

You have a fully functional RAID array!

You can now put a file system onto `/dev/md0` as you would any other device e.g.:

```
mkfs.ext4 /dev/md0
```

#### Get details about the array[](https://bash-prompt.net/guides/bash-mdadm-practice/#get-details-about-the-array)

Now that you have an array it is very useful to find information about it. This will work for any system with any RAID array on it.

##### Inspect the contents of `/proc/mdstat`[](https://bash-prompt.net/guides/bash-mdadm-practice/#inspect-the-contents-of-procmdstat)

The system keeps a file at `/proc/mdstat` that retains information about `mdadm` arrays. Use `cat` to view its contents e.g.:

```
# cat /proc/mdstat 
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid5 loop2[3] loop1[1] loop0[0]
      61440 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]
      
```

This will tell you the current status of the RAID device and the block devices that it using.

##### lsblk[](https://bash-prompt.net/guides/bash-mdadm-practice/#lsblk)

The `lsblk` command (list block devices) will show you useful information about the block devices on your system including the RAID device e.g.:

```
# lsblk -fs
NAME    FSTYPE            LABEL    UUID                                 FSAVAIL FSUSE% MOUNTPOINT
md0     ext4                       31f0183b-633c-4c65-9f61-d72b070d0851
├─loop0 linux_raid_member TEST:0   10224bb8-4f1a-7621-d5f9-8dc2c95693ad
├─loop1 linux_raid_member TEST:0   10224bb8-4f1a-7621-d5f9-8dc2c95693ad
└─loop2 linux_raid_member TEST:0   10224bb8-4f1a-7621-d5f9-8dc2c95693ad
```

##### mdadm[](https://bash-prompt.net/guides/bash-mdadm-practice/#mdadm)

The `mdadm` command will also print information about the array when it is passed the `--detail` option:

```
# mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Mon Sep  6 05:44:38 2021
        Raid Level : raid5
        Array Size : 61440 (60.00 MiB 62.91 MB)
     Used Dev Size : 30720 (30.00 MiB 31.46 MB)
      Raid Devices : 3
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Mon Sep  6 05:49:02 2021
             State : clean
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : TEST:0  (local to host TEST)
              UUID : 10224bb8:4f1a7621:d5f98dc2:c95693ad
            Events : 19

    Number   Major   Minor   RaidDevice State
       0       7        0        0      active sync   /dev/loop0
       1       7        1        1      active sync   /dev/loop1
       3       7        2        2      active sync   /dev/loop2
```
### RAID 0
### RAID 5
## Sources
https://bash-prompt.net/guides/bash-mdadm-practice/
https://www.kernel.org/doc/html/latest/admin-guide/md.html