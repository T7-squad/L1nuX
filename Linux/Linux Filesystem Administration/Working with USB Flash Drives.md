
- To list all available USB's that has been recognized by the System run the command `lsusb` :
  
  ![[Pasted image 20240129195049.png]]

- To list the partitions present on your USB drive, use the command `lsblk` :
  
  ![[Pasted image 20240129195218.png]]

- You can also use the command `df -hT` to display the Size and Filesystem Type of your USB btw Drives :
  ![[Pasted image 20240129201116.png]]

- Or you use the `mount` command to display all mounted drives on your System :
  ![[Pasted image 20240129201330.png]]

# Creating a Filesystem on USB

## Filesystem :

- Here are the common Filesystem Types :
  
  ![[Pasted image 20240129182043.png]]

### ext4 :

- To make a *ext4* Filesystem on your USB, you can use the command `mkfs` ( *make file system* ) and specify the `-t` to specify the *Type* of the Filesystem :
  
  ```SHell
[root@server1 ~] # mkfs –t ext2 /dev/sdb1
mke2fs 1.46.5 (30-Dec-2023)
Creating filesystem with 1952512 4k blocks and 488640 inodes
Filesystem UUID: 95b7fdcd-8baa-4e3f-ad6b-1ff5b5ba1f2f
Superblock backups stored on blocks:
32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632
Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done
  ```
  
### vfat :
  
- The *vfat* Format is also known as *FAT* Filesystem.
- The Fat System is universally supported.
- It is used for USB or Hard-drives that is *less* then 32GB.
- To format the USB to the vfat format :
  
  ```Shell
[root@server1 ~] # mkfs –t vfat /dev/sdb1
mkfs.fat 4.2 (2023-01-31)
  ```

### exfat :

- This Format is used for USB and Hard-drives and SSDs that are larger then *32Gb*.
- This Format is *supported* by all OS !
- Here how you do that :
  
  ```Shell
	[root@server1 ~] # mkfs –t exfat /dev/sdb1
	Creating exFAT filesystem(/dev/sdb1, cluster size=32768)
	Writing volume boot record: done
	Writing backup volume boot record: done
	Fat table creation: done
	Allocation bitmap creation: done
	Upcase table creation: done
	Writing root directory entry: done
	Synchronizing...
	exFAT format complete!
  ```


## fstab

- The `/etc/fstab` displays the drives that are mounted at boot time :
  ![[Pasted image 20240129201625.png]]

## Commands to create Filesystems 

- Here are some commands to create Filesystems :
  
  ![[Pasted image 20240129200729.png]]

## Useful Commands :

![[Pasted image 20240201122856.png]]


