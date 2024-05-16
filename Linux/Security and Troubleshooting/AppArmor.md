- Like SELinux, AppArmor provides a Mandatory Access Control (MAC).
- More in the Arch [Wiki](https://wiki.archlinux.org/title/AppArmor)
## Set Up AppArmor

### Installation

- If the `apparmor.service` doesn't seems to be enabled, you need to add the line `security=apparmor`, which will load the apparmor service at reboot :
  ![[Pasted image 20240328124222.png]]
- After that you need to update the grub with :
  
```
  sudo update-grub
```

- Install all of this related tools :
  ![[Pasted image 20240328140946.png]]


### Disable AppArmor

- To disable AppArmor, use the command :
  
```
  aa-teardown
```

- To prevent the AppArmor from booting at restart, you need to remove the line `security=apparmor` from the grub config and disable the `apparmor.service`.


## Configuring AppArmor

- All the rules are stored in the directory `/etc/apparmor.d` :
  ![[Pasted image 20240328130831.png]]

- the files that are in the form of `usr.bin.man` are the roles for the binary `/usr/bin/man`.
- You can view the policies of each file, for example we can view the rules for `usr.bin.man` :
  ![[Pasted image 20240328131112.png]]

### Display the Current Status :

- To see if the AppArmor is enabled use the command :
  
```
  aa-enabled
```

- To display the status of the AppArmor, use the command : `aa-status` :
  ![[Pasted image 20240328124825.png]]

### View the Log files 

- Like SELinux, you run view all the messages and log files of the AppArmor by installing the *audit* tool. to install audit :
  
```
  sudo apt install auditd
```

- Now is the audit tool installed and enabled.
- You can enable the audit at boot time, by adding `audit=1` in the `/etc/default/grub` :
  ![[Pasted image 20240328125759.png]]
- After that update the grub with `sudo update-grub`.

### Get a desktop notification on DENIED actions

- First create a group `audit` with the command `groupadd -r audit`.
- Second, add the current user to the group :
  
```
  gpasswd -a ubuntu audit
```

- Third, add the line `log_group = audit` in the `/etc/audit/auditd.conf`:
  ![[Pasted image 20240328131921.png]]

- Before creating the launcher, make sure that you installed `apparmor-notify` with
  
	 `apt install apparmor-notify` 

- Create a desktop launcher :
  
```bash
~/.config/autostart/apparmor-notify.desktop

[Desktop Entry]
Type=Application
Name=AppArmor Notify
Comment=Receive on screen notifications of AppArmor denials
TryExec=aa-notify
Exec=aa-notify -p -s 1 -w 60 -f /var/log/audit/audit.log
StartupNotify=false
NoDisplay=true
```


## Working with AppArmor

### List profiles

- More in the *Video* from [IT Pro TV](https://www.youtube.com/watch?v=C0lpb6mwCRE)
- To list all the apps, that are using Network Sockets, and have open Ports, AppArmor list the as *Unconfined*. to List them use the command :

```
aa-unconfined
```
![[Pasted image 20240328145645.png]]

- We have now this list of Software that are needed to be confined.

### Generating Profiles

- To generate a profile, lets say for the `/usr/sbin/rpcbind` use the command `aa-genprof /usr/sbin/rpcbind` :
  ![[Pasted image 20240328150048.png]]

- This will prompt you with questions that you need to answer.
  
### Disabling Profiles

- If you missed with a profile and you want to disable it, lets say the profile responsible for firefox  `/usr/lib/firefox-esr/firefox-esr` you can enter the command :
  
```bash
aa-disable /usr/lib/firefox-esr/firefox-esr
```


### Switching Modes

- If you want to switch to *complain mode*, use the command `aa-complain` :
  
```bash
  [root@localhost ~]# aa-complain /etc/apparmor.d/*
```

- To switch to *enforce mode* use the command `aa-enforce` :
  
```bash
  [root@localhost ~]# aa-enforce /etc/apparmor.d/*
```

- To disable a profile, use the `aa-disable` :

```bash
[root@localhost ~]# aa-disable /etc/apparmor.d/usr.bin.cupsd
Disabling /etc/apparmor.d/usr.sbin.cupsd
```