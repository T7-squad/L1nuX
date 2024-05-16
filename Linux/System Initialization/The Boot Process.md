- When the computer initializes, the system BIOS performs a *Power On Self Test (POST)*, the BIOS checks the drives for an operating system to execute.
1.  The Computer start by checking the USB, DVDs for an operating system. if it fails to find one, it checks for the *first* partition in the MBR/GPT Hard drive.

> [!NOTE] Note
> - A computer BIOS can be configured to boot an operating system from NFS, HTTP, or FTP Server across the internet, it the network interface supports the *PXE* .
> - This Process is called *netbooting*.
> - Look at the Youtube Video [https://www.youtube.com/watch?v=4btW5x_clpg](netboot_xyz)

2.  If *secure boot* is enabled in the UEFI BIOS, the digital signature of the boot loader within the UEFI System Partition is first checked to ensure that it has not been modified by malware.


> [!NOTE] Note
> The Linux kernel is stored in the /boot directory and is named vmlinux-(kernel version) if it is not ­compressed, or vmlinuz-(kernel version) if it is compressed.


3. After the linux kernel is loaded into Memory from the MBR partition, the boot loader is no longer active.
	- The kernel continue the initialization by loading the daemons into memory.

4. The first daemon that is loaded is called *init daemon* .

	- The init daemon is responsible for loading all other daemons on the system, that make the system usable and intractable for the user.

![[Pasted image 20240218194517.png]]


## Manipulate the EFI Bootloader

- To manipulate (delete,select) which Bootloader has the priority during the boot process, run the command `efibootmgr` :
  ![[Pasted image 20240312191425.png]]

- running the command `efibootmgr` :
  ![[Pasted image 20240312191531.png]]
  
  will display the Boot order.

- To delete a boot Manager, use the command `sudo efibootmgr -b 0008 --delete-bootnum`.
- To change the current boot order : `sudo efibootmgr -o 0002,0008,0001`, this will put the boot manager `0002` at the first place, `0008` the second and so on !

## Secure boot with shim EFI

- *Shim EFI* is a small EFI bootloader designed for booting linux on systems with Secure boot enabled.
- The Shim EFI is identified as `shimx64.efi` for x64 Systems.
- You can find a copy of it in the `/usr/lib/shim/shimx64.efi` :
- You can then copy it to the directory where the Shim EFI is located : `/boot/efi/EFI/debian/shimx64.efi`
![[Pasted image 20240312193856.png]]

