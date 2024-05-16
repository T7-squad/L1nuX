- *System Backup* : Make a complete backup of your system.
- *Archives* : Make a copies of your files and directories.
- *Offsite Backup* : save the copy of your files/System to another server on the internet.
- The most common backup utilities on linux :
  • **tape archive (tar)**
  • **copy in/out (cpio)**
  • **dump/restore**
  • **dd**
  • **disc burning software**

## Using tape archive (tar)

- More in [[gzip, zip, tar]].
- This archive method uses the `tar` archive command.

![[Pasted image 20240228143434.png]]


> [!Important]
> - Ta archive the Directory/File into a specific drive use the `-f` option to specify the path of the archive file : 
>   
> ```bash
> [root@server1 ~]# tar –cvf /dev/st0 *
> Desktop/
> Desktop/Home.desktop
> Desktop/trash.desktop
> samplefile
> samplefile2
> [root@server1 ~]#_
> ```
> - To view the content of your archives enter the command `tar –tvf /dev/st0` .
> - To extract them use the command : `tar –xvf /dev/st0`.
> - To append a file to the archive replace the `-c` with `-r` :  `tar –rvf /dev/st0 samplefile3`

## Using copy in/out (cpio)

### Archive

![[Pasted image 20240228145642.png]]

- To use *cpio* command, first generate a list of file/directory names with there paths with `find` :
```bash
root@Ubuntu-virt /h/u/Desktop# find  
.  
./archive  
./archive/hallo2.txt  
./archive/io.github.realmazharhussain.GdmSettings.flatpakref  
./archive/virtualbox-guest-additions-iso_7.0.10-1.dsc  
./archive/hallo.txt  
./archive/wallpaperflare.com_wallpaper.jpg  
./.directory  
./archive.tar
```

- Then pipe the find to the `cpio` command :

```bash
root@Ubuntu-virt /h/u/Desktop# find | cpio -vocB -O sample.cpio  
.  
./archive  
./archive/hallo2.txt  
./archive/io.github.realmazharhussain.GdmSettings.flatpakref  
./archive/virtualbox-guest-additions-iso_7.0.10-1.dsc  
./archive/hallo.txt  
./archive/wallpaperflare.com_wallpaper.jpg  
./.directory  
./archive.tar  
132 blocks
```

- You get then :

```bash
root@Ubuntu-virt /h/u/Desktop# ll  

-rw-r--r-- 1 root   root   660K Feb 28 14:57 sample.cpio
```

### List

- To list the content of your archive use the command `cpio –vitB –I sample.cpio` :
  
```bash
[root@server1 ~]# cpio –vitB –I sample.cpio
drwxr-xr-x
2 root root
0 Jul 27 13:40 /root/sample
-rw-r--r--
1 root root 20239 Jul 21 08:15 /root/sample/samplefile
-rw-rw-r--
1 root root
574 Jul 21 08:18 /root/sample/samplefile2
5 blocks
[root@server1 ~]#_
```

### Extract

- To extract the files from your archive use the command `cpio –vicduB -I sample.cpio` :

```bash
[root@server1 ~]# cpio –vicduB -I sample.cpio
/root/sample
/root/sample/samplefile
/root/sample/samplefile2
5 blocks
[root@server1 ~]#_
```

## Using dump/restore

- Dump only works for ext4 ,ext3 , and ext2 File systems.
- You can choose the *Full backup (0)*, or *Incremental Backup (from 1 to 3)*.
  
  ![[Pasted image 20240229121652.png]]

- Options :

![[Pasted image 20240229121729.png]]

> [!Important]
> - If you want to make a backup of A directory into your External Drive, you have to go to the location in your External Drive, where you want the backup to be stored, from there you run the command :
> ```bash
>   [root@server1 /mnt/ssd]# dump -0uf data0.dump /home/ubuntu
> ```


- use the command `dump -0uf data0.dump /dev/sda3` to dump the backup to a USB device for example. the *0* means a full backup.
     
```bash
[root@server1 ~]# dump -0uf data0.dump /dev/sda3
DUMP: Date of this level 0 dump: Sat Aug 28 10:39:12 2023
DUMP: Dumping /dev/sda3 (/data) to data0.dump
DUMP: Label: none
DUMP: Writing 10 Kilobyte records
DUMP: mapping (Pass I) [regular files]
DUMP: mapping (Pass II) [directories]
DUMP: estimated 300 blocks.
DUMP: Volume 1 started with block 1 at: Sat Aug 28 10:39:12 2023
DUMP: dumping (Pass III) [directories]
DUMP: dumping (Pass IV) [regular files]
DUMP: Closing data0.dump
DUMP: Volume 1 completed at: Sat Aug 28 10:39:12 2023
DUMP: Volume 1 290 blocks (0.28MB)
DUMP: 290 blocks (0.28MB) on 1 volume(s)
DUMP: finished in less than a second
DUMP: Date of this level 0 dump: Sat Aug 28 10:39:12 2023
DUMP: Date this dump completed: Sat Aug 28 10:39:12 2023
DUMP: Average transfer rate: 340 kB/s
DUMP: DUMP IS DONE
[root@server1 ~]#_ 
```

- You can add the mounting point, instead of the USB device :
  
```bash
dump -0uf data0.dump /data
```

- To create a dump of a specific device you can use the following command : 

```bash
dump -0uf /dev/st0 /data
```

### List the Data

- To view the dump archive look for the `/etc/dumpdates` :
  
```bash
[root@server1 ~]# cat /etc/dumpdates
/dev/sda3 0 Sat Aug 28 10:39:12 2023 -0400
```

- To view the content of the data enter the command `restore -tf data0.dump`.

### Restore the Data

- To restore the Data enter the command `restore –vrf data0.dump` :
  
```bash
[root@server1 ~]# restore –vrf data0.dump
Verify tape and initialize maps
Input is from a local file/pipe
Input block size is 32
Dump
date: Sat Aug 28 10:39:12 2023
Dumped from: the epoch
Level 0 dump of /data on server1:/dev/sda3
Label: none
Begin level 0 restore
Initialize symbol table.
Extract directories from data0.dump
Calculate extraction list.
Make node ./lost+found
Extract new leaves.
Check pointing the restore
extract file ./inittab
extract file ./hosts
extract file ./issue
extract file ./aquota.group
extract file ./aquota.user
Add links
Set directory mode, owner, and times.
Check the symbol table.
Check pointing the re
```

## dd

- The `dd` command back up data block by block to an archive device or file.
- Its used to make an *image backup* of the entire system.

```bash
[root@server1 ~]# dd if=/dev/sda2 of=/sda2.img bs=1M
2097152+0 records in
2097152+0 records out
1073741824 bytes (1.1 GB, 1.0 GiB) copied, 9.29465 s, 116 MB/s
[root@server1 ~]#_
```

- To restore the filesystem on `/dev/sda2` from `/sda2.img` you need to unmount the device `/dev/sda2` and then run the command `dd if=/sda2.img of=/dev/sda2 bs=1M` :
  
```bash
[root@server1 ~]# umount /dev/sda2
[root@server1 ~]# dd if=/sda2.img of=/dev/sda2
2097152+0 records in
2097152+0 records out
1073741824 bytes (1.1 GB, 1.0 GiB) copied, 44.7753 s, 24.0 MB/s
[root@server1 ~]#_
```

- To store the *MBR* System use the command : 
  
```
  dd if=/dev/sda of=/MBRarchive.img bs=512 count=1
```

> [!Important]
> - dd don't backup Directories !!

