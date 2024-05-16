### What is causing the corruption of the Filesystem ?

- The most common filesystem corruption occurs because a system was not shut down properly using the *shutdown, poweroff, halt, or reboot* commands. Data is stored in memory for a short period of time before it is written to a file on the filesystem. This process of saving data to the file-system is called *syncing*. If the computer’s power is turned off, data in memory might not be synced properly to the filesystem, causing corruption.
- Filesystem corruption can also occur if the storage devices are used frequently for time-intensive tasks such as database access. As the usage of any system increases, so does the possibility for operating system errors when writing to storage devices. Along the same lines, physical hard disk drives and SSDs themselves can wear over time with heavy usage. Some parts of a hard disk drive platter may cease to hold a magnetic charge and some of the NAND flash memory cells in an SSD may cease to function properly; these areas are known as bad blocks. When the operating system finds a *bad block*, it puts a reference to the block in the bad blocks table on the filesystem. Any entries in the bad blocks table are not used for any future storage.

# fsck

- `fsck` is the *filesystem check* which checks the hard-drive for errors.
- ==! To check if a disk contains error, you need to first UNMOUNT the drive !== :

1. 
   
  ```bash
	[root@server1 ~]# fsck /dev/vg00/data1
	fsck from util-linux 2.38
	e2fsck 1.46.5 (30-Dec-2023)
	/dev/mapper/vg00-data1 is mounted.
	e2fsck: Cannot continue, aborting. 
```

2. Unmount the Disk :

```bash
[root@server1 ~]# umount /dev/vg00/data1
```

3. run `fsck` :

```bash
[root@server1 ~]# fsck /dev/vg00/data1
fsck from util-linux 2.38
e2fsck 1.46.5 (30-Dec-2023)
/dev/mapper/vg00-data1: clean, 11/655360 files, 66753/2621440 blocks
```


> [!NOTE] fsck the root partition
> Because the root filesystem cannot be unmounted, you should only run the fsck command on the root
filesystem from [[Single User Mode]] or from a system booted from live installation media.


![[Pasted image 20240201184518.png]]

## fsck Commands 

![[Pasted image 20240201184611.png]]

- `-r` : fsck report statistics.
  ![[Pasted image 20240506165144.png]]
# Badblocks

- `Badblock` is also like `fsck` schecks for badblocks in your hard-drive.
- ==Before you lunch this command you need to enter the *Single User Mode==
- See [[Single User Mode]] .

![[Pasted image 20240201183332.png]]

# e2fsck

- The `e2fsck` Tool is specialy used for the *ext2,3,4* Disk Format !

```Bash

[root@server1 ~]# e2fsck -c /dev/vg00/data1
e2fsck 1.46.5 (30-Dec-2023)
Checking for bad blocks (read-only test): done
/dev/vg00/data1: Updating bad block inode.
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/vg00/data1: ***** FILE SYSTEM WAS MODIFIED *****
/dev/vg00/data1: 11/655360 files (0.0% non-contiguous), 66753/2621440 blocks

```


# tune2fs

- This command check the *ext2,3,4* Filesystems.
- You can list the informations about a partition by entering the command with the option `-l` to list the informations  :
  
```bash
  sudo tune2fs -l /dev/dev/nvme0n1p2
```
![[Pasted image 20240426180106.png]]

- You can program a filesystem check any time you want. To do that you need to add the option `-i 10` 10 is for 10 days :
  
```bash
sudo tune2fs -i 10 /dev/nvme0n1p2
```

- Now is the next check in 10 days (*!on Boot!*):
  ![[Pasted image 20240426180442.png]]

- You can also set the *Maximum mount count* (maximum reboot times) after which the command can also be lanched :
  
```bash
  sudo tune2fs -c 5 /dev/nvme0n1p2
```

![[Pasted image 20240426181212.png]]



