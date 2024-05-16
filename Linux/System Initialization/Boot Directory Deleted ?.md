
Boot directory has been removed :

1. `sudo apt-get install --reinstall linux-image-$(uname -r)`
2. `sudo apt-get install linux-source`
3. `grub-mkconfig -o /boot/grub2/grub.cfg`
4. `apt-get install --reinstall grub-efi-amd64`            (efi)
5.  if you get this error :

```
# sudo update-initramfs -k 6.5.0-17-generic -c

update-initramfs: Generating /boot/initrd.img-6.5.0-17-generic
W: Kernel configuration /boot/config-6.5.0-17-generic is missing, cannot check for zstd compression support (CONFIG_RD_ZSTD)
```

go to `/usr/share/` search for the folder `linux-headers-6.5.0-17`, cd to it and copy the `.config` file to `/boot/config-6.5.0-17-generic` :

```
cp .config /boot/config-6.5.0-17-generic
```

Now update your initramfs :

```
root@Ubuntu-virt /u/s/linux-headers-6.5.0-17-generic# sudo update-initramfs -k 6.5.0-17-generic -c


update-initramfs: Generating /boot/initrd.img-6.5.0-17-generic

```

6. *copy the Grub files* : to prevent the Error *# file ‘/grub/i386-pc/normal.mod’ not found* at boot time, you need to copy the grub files to the `/boot/grub` directory :

```
sudo cp -r /usr/lib/grub/* /boot/grub/

```