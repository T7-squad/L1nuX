- The `/dev` Directory is where the hard drives, USB, terminals and serial ports are located.
- The `/dev` Directory is composed of 2 Devices :
  - *Character Device :* Transfer Data character by character (ttys).
  - *Block Device :* Transfer a chunk of Data by using the physical memory to buffer the transfer (sda/sdb...).
## Common Device Files :

- Here are the most common Device Files in Linux :

![[Pasted image 20240129180816.png]]
![[Pasted image 20240129180829.png]]

- To see The major and minor Numbers of the Devices you can run the command `/proc/devices` :
  
  ```Shell
  [root@server1 ~] # cat /proc/devices
Character devices:
1 mem
4 /dev/vc/0
4 tty
4 ttyS
5 /dev/tty
5 /dev/console
5 /dev/ptmx
7 vcs
10 misc
13 input
14 sound
21 sg
29 fb
116 alsa
128 ptm
136 pts
162 raw
180 usb
188 ttyUSB
189 usb_device
202 cpu/msr
203 cpu/cpuid
240 usbmon
Block devices:
8 sd
9 md
11 sr
65 sd
128 sd
252 zram
253 device-mapper
254 mdp
259 blkext
  ```

- You can also list all the devices (USB, hard drives ...) by running `lsblk` :
  
![[Pasted image 20240129181429.png]]


   