
- The primary function of the boot loaders is to load the kernel into memory.
- The boot loader can also perform a *multi booting*, and loads different operating systems on user input.

# GRUB Legacy (GRUB 1)

- The GRUB Legacy boot loader resides on the MBR Partition.
- The GRUB boot loader displays the boot screen by lunching the binaries under */boot/grub* :
  ![[Pasted image 20240218201034.png]]

> [!Important]
> - You can also find the copy of the Bootloader under the `/usr/lib/grub` :
>   ![[Pasted image 20240312192838.png]]
> - If you happened to mess with the grub Bootloader, you can copy it to `/boot/grub` directory !

- The boot screen will look something like :
  ![[Pasted image 20240218201143.png]]
- You can edit the GRUB legacy loader by editing the file `/boot/grub/grub.conf` :
  
```bash
[root@server1 ~] # cat /boot/grub/grub.conf
# NOTE: You do not have a /boot partition. This means that all
# kernel and initrd paths are relative to /, or root (hd0,0)
boot=/dev/sda
default=0
timeout=5
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
hiddenmenu
title Fedora (2.6.33.3-85.fc13.i686.PAE)
root (hd0,0)
kernel /boot/vmlinuz-2.6.fc13.i686.PAE ro root=/dev/sda1 rhgb quiet
initrd /boot/initramfs-2.6.fc13.i686.PAE.img
[root@server1 ~]#_
```

- To understand the entries of the grub.conf, you need to understand how grub refers to the partitions on hard drives.
  - Hard drives are numbered on the following format : *(hd<drive#>,<partition#>).*
    - `(hd0,0)` : refers to the first hard drive and the first partition on the system (/boot or /root)
    - `(hd0,1)` : refers to the first hard drive and the second partition on the system.
    - `(hd2,3)` : fourth partition on the third hard drive.
- Normally the partition that contains the /boot directory is the /root directory.
- The example /boot/grub/grub.conf file shown earlier displays a graphical boot screen(`splashimage=(hd0,0)/boot/grub/splash.xpm.gz`) and boots the default operating system kernel on the first hard drive (`default=0`) in 5 seconds (`timeout=5`) without showing any additional menus `(hiddenmenu`). The default operating system kernel is located on the GRUB root filesystem (`root (hd0,0)`) and is called `/boot/vmlinuz-2.6.fc13.i686.PAE`.
- The kernel then mounts the root file system on /dev/sda1 (`root=/dev/sda1`) initially as read-only (`ro`) to avoid problems with the fsck command and uses a special initramfs disk filesystem image to load modules into RAM that are needed by the Linux kernel at boot time ( `initrd /boot/initramfs-2.6.fc13.i686.PAE.img`).


> [!NOTE] Note
> Most Linux distributions use a UUID (e.g., `root=UUID=42c0fce6-bb79-4218-af1a-0b89316bb7d1`)
in place of `root=/dev/sda1` to identify the partition that holds the root filesystem at boot time.

- Normally, GRUB Legacy allows users to manipulate the boot loader during system startup; to prevent this, you can optionally password protect GRUB modifications during boot time.
- If the GRUB boot loader becomes damaged, you can reinstall it using the `grub-install command` that is available, when you reboot on a live USB.
  
```bash
[root@server1 ~]# grub-install /dev/sda
Installation finished. No error reported.
Below are the contents of the device map /boot/grub/device.map.
If lines are incorrect, fix them and re-run the script ’grub-install'.
(hd0)
/dev/sda
[root@server1 ~]#_
```

# GRUB 2

- GRUB 2 is the boot loader commonly used by modern linux systems, it supports newer storage devices as NVMe.
- GRUB 2 has an additional support for systems that do not use the x_86_64 architecture.
- *For a System that uses a standard BIOS, GRUB 2 operate the same way as GRUB 1.*
- *For a System that uses a UEFI/BIOS* all stages are stored in the *UEFI* system partition with an efi appliction called `grubx64.efi`.
  - The UEFI file system is mounted to `/boot/efi` directory during the installation :
    ![[Pasted image 20240218205239.png]]

![[Pasted image 20240218205341.png]]

- After the `grubx64.efi` is executed by the GRUB2, you will get the same GRUB screen as above.
- The main configuration file for the GRUB2 is the `grub.cfg` file present in the `/boot/grub` and under the `/boot/efi/EFI/debian/` directory :
  ![[Pasted image 20240218205738.png]]![[Pasted image 20240218205946.png]]

- The `grubx64.efi` of the GRUB 2 is different then the one of the GRUB 1 :
  
```bash
set timeout=5

menuentry 'Fedora (5.17.5-300.fc36.x86_64) 36 (Workstation Edition)' --class fedora --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-5.17.5-300.fc36.x86_64-advanced-9ff25add-035b-4d26-9129-a9da6d0a6fa3' {
	
	load_video
	set gfxpayload=keep
	insmod gzio
	insmod part_gpt
	insmod ext2
	set root='hd0,gpt2'

linuxefi /vmlinuz-5.17.5-300.fc36.x86_64 root=UUID=9ff25add-035b-4d26-9129-a9da6d0a6fa3 ro resume=UUID=4837de7b-d96e-4edc-8d3d-56df0a17ca62 rhgb quiet LANG=en_US.UTF-8

initrdefi /initramfs-5.17.5-300.fc36.x86_64.img
}
```

- The boot loader uses the command `insmod` to load the required hardware before accessing the root partition.
- The partition number starts with 1 and are prefixed with *msdos* for MBR and *gpt* for GPT partitions.
- The set `root='hd0,gpt2'` notation in the preceding excerpt indicates that the GRUB root partition is the second GPT partition on the first hard disk drive.
- the use of a graphical boot screen (`rhgb`) that suppresses errors from being shown during the boot process (`quiet`).
- `initrdefi /initramfs-5.17.5-300.fc36.x86_64.img` the init loads the kernel modules into RAM.
- to edit the GRUB 2 config file, you can edit the file : `cat /etc/default/grub` :
  
```bash
[root@server1 ~]# cat /etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="rhgb quiet"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG="
```

- After editing the GRUB 2 config file, you *MUS* reload the config file :
  
```bash
[root@server1 ~] grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file …
done
[root@server1 ~]#_
```

- On Systems with UEFI/BIOS you can instead use the command :
  
```
  grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

- If the GRUB become damaged, you can reinstall it using the command :
  
```bash
[root@server1 ~] grub2-install /dev/sda
Installation finished. No error reported.
[root@server1 ~]#_
```

- For systems with UEFI BIOS you can reinstall GRUB 2 with the command (on debian) :
  
```bash
  sudo apt-get install --reinstall grub-efi
```


> [!NOTE] Note
> For simplicity, some Linux distributions use `grub-mkconfig`, `grub-install` and update-grub in place of `grub2-mkconfig`, `grub2-install` and `update-grub2`, respectively



> [!NOTE] Important
> When you update your Linux operating system software, you may receive an updated version of your distribution kernel. In this case, the software update copies the new kernel to the /boot directory, creates a new initramfs to match the new kernel, and modifies the GRUB2 configuration to ensure that the new kernel is listed at the top of the graphical boot menu and set as the default for subsequent boot processes.



> [!NOTE] Important
> Invalid entries in the GRUB2 configuration file or a damaged initramfs can prevent the kernel from loading successfully; this is called a *kernel panic* and will result in a system halt immediately after GRUB attempts to load the Linux kernel. In this case, you will need to edit the GRUB2 configuration file from a live Linux system used for system rescue or generate a new initramfs using either the `dracut` command or `mkinitrd` command.
> - For Dracut : `sudo dracut --hostonly --no-hostonly-cmdline /boot/initramfs-$(uname -r).img --force` . 
> - For Initramfs : `sudo update-initramfs -c -k $(uname -r)` :
>   - `c` : to create.
>   - `k` : speficy the kernel version.

