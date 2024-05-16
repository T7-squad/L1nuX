## Time
### Hardware Clock

- The system obtains the current time from its current *hardware clock* or BIOS Clock.
- You need first to install the package `sudo apt install util-linux-extra`.
- Know you can run the command `sudo hwclock --verbose` :

```bash
ubuntu@Ubuntu-virt ~ [1]> sudo hwclock --verbose
hwclock from util-linux 2.39.1
System Time: 1708599538.630972
Trying to open: /dev/rtc0
Using the rtc interface to the clock.
Assuming hardware clock is kept in UTC time.
Waiting for clock tick...
...got clock tick
Time read from Hardware Clock: 2024/02/22 10:58:59
Hw clock time : 2024/02/22 10:58:59 = 1708599539 seconds since 1969
Time since last adjustment is 1708599539 seconds
Calculated Hardware Clock drift is 0.000000 seconds
2024-02-22 11:58:58.245906+01:00
```

- To display the current day run the command : `date`.
- To Set up a new Date and modify the current BIOS Date to 2023-08-20 08:16:00 :
  
```
hwclock --set --date='2023-08-20 08:16:00
```

### Software Clock

- To modify the Software (System) Clock you run the command : `date -s "20 AUG 2023 08:16:00"`.
- You can after that synchronize the BIOS time to match the Linux current Time with : `hwclock -w`.

### Localtime

- the localtime is located in the `/etc/localtime` :
  ![[Pasted image 20240222121401.png]]

- The actual timezone directory is located in the `/usr/share/zoneinfo` directory :
  ![[Pasted image 20240222121739.png]]

- To modify the current timezone, you need to modify the link to point to a Timezone in `/usr/share/timezone`. In this example we set the timezone to Toronto, `-f` to force create the link :

```bash
sudo ln -sf /usr/share/zoneinfo/America/Toronto /etc/localtime
```

- This will set the Timezone automaticly :

![[Pasted image 20240222122713.png]]


### Timezone

1. 

- To see the default timezone in your System, you run the command `cat /etc/timezone` :
  
  ![[Pasted image 20240222123153.png]]

- To change the current timezone, you can use the command `tzselect`. This will prompt you with questions that you need to answer in order for you to set the timezone of your contry :
  
  ![[Pasted image 20240222123336.png]]![[Pasted image 20240222123358.png]]

- Now we have set a new timezone !

2. 

- Another option is to use the command `timedatectl` :
  
  ![[Pasted image 20240222123612.png]]

- You can also modify the time by running the command : `timedatectl set-time "2023-08-20 08:16:00"`
- Or modify the timezone with : `timedatectl set-timezone ’America/Toronto’`.
- Or use NTP to define the timezone : `timedatectlset-ntp true` .

## Format (Language)

- To display which languange is used for the linux kernel, use the command : `cat /etc/locale.conf` :
  ![[Pasted image 20240222130045.png]]
  
  - Or the command : `locale` 
    ![[Pasted image 20240222125948.png]]

  - Or the command : `localectl` :
    ![[Pasted image 20240222130213.png]]

- To list all available locale in your Linux System, enter the command : `locale -a` :
  ![[Pasted image 20240222130339.png]]

- To set the local to another one for example `en_CA.UTF-8` , you run the command `localectl set-locale LANG=en_CA.UTF-8`.

### Set the Keyboard to de

- One way to do that is with the command : `sudo dpkg-reconfigure keyboard-configuration`.
- Another methode is to modify the `KEYMAP=de` in `/etc/vconsole.conf` directory .
