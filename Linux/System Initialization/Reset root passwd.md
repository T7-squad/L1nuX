## Method 1

1. Enter the recovery mode by entering the grub menu and adding the `single` or `init=/bin/bash` to boot into single-user mode.
2. Remout the root directory with :
   
```bash
   mount -o remount,rw /
```

3. Reset the Password :

```
  passwd root
```

4. Reboot !

![[Pasted image 20240415211802.png]]

## Method 2

- Boot up from a live usb, mount the files, chroot to the root partition and change the password.
