- Hard Drives comes in 3 Shapes :
  - PATA.
  - SATA.
  - SCSI/SAS.
- Device hierarchy of a Hard Drive :
  - *Primary Master : /dev/hda. *
  - *Primary Slave : /dev/hdb. *
  - *Secondary Master : /dev/hdc. *
  - *Secondary Slave : /dev/hdd. *
- With the new NVMes SSDs the Directory Tree now looks like this :
  - */dev/nwme0* : is the First NVMe SSD. 
    - */dev/nvme0n1p1* : is the first Partition of the First NVMe SSD.
    - *dev/nvme0n1p2*: is the second Partition of the First NVMe SSD.
      ![[Pasted image 20240201124222.png]]
      
  - */dev/nwme1* : is the Second NVMe SSD. 
  - */dev/nwme2* : is the third NVMe SSD.


# Theory of Hard Disk Drive Partitioning 

- Common MBR partition device files :
  ![[Pasted image 20240201124818.png]]

- A simple MBR partition strategy :

![[Pasted image 20240201124942.png]]

- For the dual Windows/Linux Boot you need to create a multi or dual-boot partition :

![[Pasted image 20240201125114.png]]


# Command Line Hard Drive Patitioning 

- First you choose your hard drive. For example `/dev/sda` .
- Enter the `sudo fdisk /dev/sda` :
  
  ```Bash
[root@server1 ~] # fdisk /dev/sda
	Welcome to fdisk (util-linux 2.38).
	Changes will remain in memory only, until you decide to write them.
	Be careful before using the write command.
	Command (m for help):
  ```

## Partitioning :

- Lets Partition out device as following :
  
  ![[Pasted image 20240201131533.png]]
  
### 1. Create our Partitions 

- To partition your hard drive enter the command `p` :
  
  ```Bash
Command (m for help): p
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt <---- ## Disk Label !
Disk identifier: 6DE393B9-5DBA-466F-AF72-1D79894BBFEB
Device
Start
End Sectors Size Type
/dev/sda1
2048
1230847 1228800 600M EFI System
/dev/sda2
1230848
3327999 2097152
1G Linux filesystem
/dev/sda3
3328000 76728319 73400320
35G Linux filesystem
/dev/sda4 76728320 104855551 28127232 13.4G Linux filesystem
Command (m for help):_
  ```
  
  - In this exemple is the disk label `gpt`.
  - For `MBR` storage devices the disk label would be *msdos*.

- To delete a partition, enter the command `d` :
  
  ```Bash
Command (m for help): d
Partition number (1-4, default 4): 4
Partition 4 has been deleted.
  ```
  
  - In this example, we deleted the partition 4.

- To print the partition table, press enter the command `p` :
  
  ```Bash
Command (m for help): p
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 6DE393B9-5DBA-466F-AF72-1D79894BBFEB
Device
Start
End Sectors Size Type
/dev/sda1 2048 1230847 1228800 600M EFI System
/dev/sda2 1230848 3327999 2097152 1G Linux filesystem
/dev/sda3 3328000 76728319 73400320 35G Linux filesystem
/dev/sda4 76728320 1048
  ```

- To create an aditional partition, enter the command `n` :
  
  ```Bash
  Command (m for help): n
Partition number (4-128, default 4): 4
First sector (76728320-104857566, default 76728320): 76728320
Last sector, +/-sectors or +/-size{K,M,G,T,P} (76728320-104857566,
default 104855551): +5G <---- # MAKE A 5GB PARTITION
Created a new partition 4 of type 'Linux filesystem' and of size 5 GiB.
Command (m for help): n
Partition number (5-128, default 5): 5
First sector (87214080-104857566, default 87214080): 87214080
Last sector, +/-sectors or +/-size{K,M,G,T,P} (87214080-104857566,
default 104855551): 104855551
Created a new partition 5 of type 'Linux filesystem' and of size 8.4 GiB.
  ```

- Finaly you should write the changes to disk with the command `w` :
  
  ```Bash
Command (m for help): w
The partition table has been altered.
Syncing disks. [root@server1 ~]#_
  ```

- The Best is at the End you should run `partprobe command` to inform the Kernel of the hard disk changes you made.

### 2. Create a Filesystem

- Look also at [[Working with USB Flash Drives]].

#### ext4 for /home and /

- To create a ext4 partition you need to run the command `mkfs -t ext4 /dev/sda5` :
- In this examples we use the tool `fdisk` and `mkfs` .
  ![[Pasted image 20240201132410.png]]

![[Pasted image 20240201133422.png]]

#### swap for /dev/sda4 

- To create a swap filesystem you need to run the command `mkswap /dev/sda4` and activate the swap with `swapon /dev/sda4` :
  ![[Pasted image 20240201132604.png]]
  ![[Pasted image 20240415205953.png]]
  
- To disactivate the swap partition enter the command `swapoff command`.
  ![[Pasted image 20240415205916.png]]

- To see the free Space in the swap, run the command :
  
```
  sudo free
```

![[Pasted image 20240415210106.png]]


#### ext4 for /boot

#### vfat for /boot/efi

![[Pasted image 20240201133325.png]]

- To check your changes enter the `cat /etc/fstab` :
  ![[Pasted image 20240201133045.png]]

