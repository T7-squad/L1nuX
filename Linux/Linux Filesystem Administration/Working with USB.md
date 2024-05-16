## Mounting and Unmounting Storage Devices

- The first thig to do with the storage device is to attach it to the systemtree. this mechanism is called `mounting` ; its makes the device communicate with the operating system.
- You can use the `lsblk` command to see a list of all the block devices connected to your Linux system:
	
	`lsblk`
	
	This will show you a list of all the block devices, including hard drives, USB drives, and other storage devices, along with information such as their size, type, and mounted file systems.

	![image](found.png)

- You can also use the `df` command to see information about the file systems that are mounted on your system, including the device name, mount point, and the amount of space used and available:
	
	`df -h`
	
	This will show you the file system disk space usage, displayed in human-readable format (e.g., with "G" for gigabytes, "M" for megabytes, etc.)
	
	Macos :
	![image](LA.png)

	Linux :
	![image](df.png)

### See All mounted Devices 

- run the command `mount` on Linux/Macos
	- Linux :
	 ![image](mount.png)
	 - Macos :
	 ![image](mc.png)

### Determine Device Names 

![image](dev.png)

### How to do it 

To mount a USB drive:

1.  First, check the name and partition of the USB drive by using the `lsblk` command.
2.  Create a mount point directory, which is the location where the USB drive will be accessible after it is mounted. For example: `sudo mkdir /mnt/usb`.
3.  Use the `mount` command with the appropriate options to mount the USB drive to the mount point. For example: `sudo mount /dev/sdb1 /mnt/usb`.

To unmount a USB drive:

1.  First, check the name of the mounted USB drive by using the `df -h` command.
2.  Use the `umount` command with the appropriate options to unmount the USB drive from the file system. For example: `sudo umount /mnt/usb`.

Note: Be cautious when mounting and unmounting USB drives, as incorrect usage can result in data loss or corruption.

![image](mnt.png)

### Changing File Format

- type the command `sudo fdisk /dev/sdb2` on the shell. `/dev/sdb2` should be your USB Drive.
- for more help type the `m` option
- the `p` option gives you all partition types :

![image](type.png)

This table shows the different partition types (also known as "partition ID") and what they are typically used for. Here are the most common partition types and their uses:

-   `0x01` (`FAT12`): An older file system used for floppy disks.
    
-   `0x04` (`FAT16`): A file system commonly used for USB drives and older hard drives.
    
-   `0x06` (`FAT16`): A file system commonly used for USB drives and older hard drives.
    
-   `0x07` (`HPFS/NTFS/exFAT`): The NTFS file system used by modern Windows operating systems.
    
-   `0x0b` (`W95 FAT32`): The FAT32 file system commonly used for USB drives.
    
-   `0x0c` (`W95 FAT32 (LBA)`): An extension of the FAT32 file system that supports larger drives.
    
-   `0x0e` (`W95 FAT16 (LBA)`): An extension of the FAT16 file system that supports larger drives.
    
-   `0x0f` (`W95 Ext'd (LBA)`): A type of extended partition used in older Windows systems.
    
-   `0x83` (`Linux`): The default file system used by Linux operating systems.
    
-   `0x82` (`Linux swap / Solaris`): A partition used as virtual memory in Linux or as a swap partition in Solaris.
    
-   `0x8e` (`Linux LVM`): A partition used for Logical Volume Management in Linux.
    
-   `0xfd` (`Linux raid auto`): A partition used for automatic RAID detection and configuration in Linux.
    
-   `0xee` (`GPT`): The partition table format used by modern computers.
    
-   `0xef` (`EFI (FAT-12/16/32)`): A partition used for booting on computers using the UEFI firmware.
    

- `NTFS` (`01 Hidden NTFS Win`) and `exFAT` (`07 HPFS/NTFS/exFAT`) are the most compatible file systems with MacOS, Windows, and Linux

### Formating

- Check [formating_fdisk_mkfs_Youtube](https://www.youtube.com/watch?v=GytPlfaEIBY), [formating_Usb_in_Linux_Youtube](https://www.youtube.com/watch?v=DflWASw7JMU)
- There are 3 Steps to format the USB :
	1- Go to `fdsik /dev/[device_name]`
	- Delete all partitions
	2- Create new ones with the correct partition type
	- To format your USB, you need to find the propre File Format.
	- Umount your USB `sudo unmount /dev/[device_name]`
	- For Exemple to format to NTFS you need to type the command ``sudo mkfs.ntfs /dev/[device-name]``
	- If your formating was successfull youll get this flag :
	
	 ![image|500](for.png)
	 
	3- check your USB with `fsck /dev/[device_name]`
	See [formating_Usb_in_Linux_Youtube](https://www.youtube.com/watch?v=DflWASw7JMU)
- At the End you get  the new formated USB:

![image](usb.png)
