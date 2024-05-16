# How to enter the Single User Mode ?

- There are Multiple Examples for that. One of them is to boot up your PC, keep pressing the `Shift` button and enter the Grub Menu :
  
  ![[Pasted image 20240201180859.png]]
  
- After that, you press `e` to edit the commands before booting.
- add `init=/bin/sh` at the end of the Line, where the kernl in loaded :
  
  ![[Pasted image 20240201181130.png]]

- After that Press `Ctrl-X` to boot directly with these changes.
- now we have a root shell :
  ![[Pasted image 20240201181302.png]]

- ~~Now you are in the Single User Mode !~~


> [!NOTE] Change Keyboard Language && Shell
> - Under the Singel User Mode, use the command `loadkeys de` to change the shell language from English to German for example.
> - You can use the `/bin/bash` Shell instead of the `/bin/sh`. It makes your life more easier !

# How The Single User Mode can be Useful ?

## File System Repairs

- When you access the single user mode, the partitions are not mounted or are mounted in a *read only* mode. 
- In this case you can run `fsck` to repair you *root* or your complete hard drive.
- Look at [[Checking Filesystems for Errors]] .

