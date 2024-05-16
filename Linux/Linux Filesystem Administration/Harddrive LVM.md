# LVM Management

## 1. Initializing the Volume for the lvm conversion

- First you need to choose to the volume and initialize it to an lvm Conversion.
- To do that you need to run the command `pvcreate /dev/sdb1` for example.
- run `pvdisplay` to display the content :
  ![[Pasted image 20240202172622.png]]
  
## 2. Create the LVM Group 

- Next you need to create an LVM group.
- To do that run the command : `vgcreate vg00 /dev/sdb1` .
- This will create the `vg00` LVM group and add the `/dev/sdb1` to it.
- To display the group, enter the command : `lvdisplay` .

## 3. Create a logical Volume withing The group

- Next, you need to create the logical volume within the groupe.
- Enter the command : `lvcreate –L 0.9GB –n newdata vg00` :
  - `-L 0.9GB` : will create 0.9 GB of Volume.
  - `-n newdata` : is the name of the volume inside the group.
  - `vg00` : is the group Name.
- `lvdisplay` to display the changes :
  
  ![[Pasted image 20240202173542.png]]

## 4. Add the Disk Format for your Volume

- Now you need to add a disk format to your volume.
- Lets take the *ext4* Example
- To do that enter the command : `mkfs –t ext4 /dev/vg00/newdata`.
- now you need to mount the lvm drive :
  - create a directory : `mkdir /newdata` .
  - mount the drive : `mount /dev/vg00/newdata /newdata`.


## 4. Resize the LVM Disk 

- To resize your LVM disk, Enter the command : `lvextend -L +500MB -r /dev/vg00/newdata` :
  - `-r` : flag stands for resize.

![[Pasted image 20240202184125.png]]

- `lvdisplay` :
  
![[Pasted image 20240202184155.png]]

![[Pasted image 20240202184246.png]]

 
## 5. Remove an LVM disk from the Group

- Enter the command : `lvremove /dev/vg00/newdata1 -ff` this will remove the lvm */dev/newdata1* .

> [!NOTE] !ACHTUNG!
> Don't use `pvremove` ! this will attempt to remove the hole device !

# Add More LVM

## Add the second LVM

- To add a second LVM, use the command `lvcreate -L 500MB -n newdata1 vg00`.
  - `-L 500MB` : is the size of the second pv.
  - `-n newdata1` : is the name of the new pv.
  - `vg00` : is the lvm group.
- Display :
  ![[Pasted image 20240202182126.png]]

## Add the disk Format to the new Drive

- Enter the command : `mkfs -t ext4 /dev/vg00/newdata1` .
- This will convert the drive to ext4 :
  ![[Pasted image 20240202182100.png]]

- The Output :
  ![[Pasted image 20240202182343.png]]


  