#### `/proc` :

- After installing your linux distribution, you muss be sure that all the drivers are working. one of the methods to check for the hardware is to look at the `/proc` directory that list hardware for the kernel within RAM :
  ![[Pasted image 20240218154759.png]]

- What you can commonly find withing the `/proc` directory :
  
  ![[Pasted image 20240218155458.png]]

#### `/sys` :

- This directory lists all devices on the system that is used by commands and other software.
- The /sys directory lists also the block devices that were detected by the system, as well as the detailed configuration for sda :

![[Pasted image 20240218162309.png]]

- The `block` directory inside the sys directory lists all sda available :
  ![[Pasted image 20240218162417.png]]

### Linux Commands that Display Hardware Information


> [!Important]
> - One of the most important command is the `udevadm monitor` command.
> - This command enable you to see track the the Kernel interraction with the new drives Live :
>   ![[Pasted image 20240408184024.png]]
> 

  
1. `lscpu` :
   - Displays all the information about the CPU :
     
     ![[Pasted image 20240218162756.png]]

2. `lsmem` :
   -  Displays all the Informations about the RAM :    
     ![[Pasted image 20240218163001.png]]
 
3. `lspci -v` :
   - Displays information about connected PCI controllers and devices on the system withe there kernel drivers :
     ![[Pasted image 20240218163216.png]]

4. `lsusb` :
   - List all available and connected USB Devices :
     ![[Pasted image 20240218163701.png]]
   - To get more informations, use the `v` flag.
5. `lshw` :
   - Displays general hardware information for the entire system as detected by the Linux kernel.
   - lshw is not a present command on linux, you have to install it with `sudo apt-get install lshw -y`.
   - You can print the output of the lshw as html with : `⋊  lshw -html > system.html`
     ![[Pasted image 20240218165035.png]]
   

6. `dmidecode` :
   - Display the hardware informations as detected by the BIOS/UEFI.
7. `hwinfo` :
   - `hwinfo` is the same as lshw, it display the information about the hardware devices on the system.
   - `hwinfo` is not pre-installed in Linux, you have to install it manually by :  `sudo apt-get install hwinfo` .
   - you can then choose your interface which you want more informations about :
     ![[Pasted image 20240218165925.png]]
     
  - After that you can enter `hwinfo --HARDWARE_ITEM` like `hwinfo --wlan` :
    ![[Pasted image 20240218170135.png]]

8. `dmesg` :
   - Display the Kernel ring buffer (RAM).
   - Use the command `dmesg -xe -T` to display human readable time and object oriented Messages :
     ![[Pasted image 20240218170708.png]]

9. `journalctl` :
   - `journalctl` is a command capable of printing out all events accrued since the boot of the kernel.
   - journalctl display informations that is easy to read.
   - `journalctl -r` : `-r` for recursive will display the latest messages first.
   - `journalctl -b` : will display the messages from the boot start.
   - `journalctl --since=today` : will display all the information available today.
     ![[Pasted image 20240218171829.png]]
