
# Connect to Wlan through command GUI

- If you want to connect to a Wlan on the GUI Terminal, type the command `nmtui`:
![[Pasted image 20240413182203.png]]

- Press `Edit a connection` to select your Wifi Interface and connect to it :
  ![[Pasted image 20240413182309.png]]


# Display Informations about the wlan interface

- To display the informations about the wlan interface `wlo1`, use the command `iwlist` :
  ![[Pasted image 20240408115319.png]]
- Here you can chose your information.
- Lets say you want to check your ==Frequency== :
  ![[Pasted image 20240408115429.png]]

  In this case you use the command `iwlist wlo1 frequency` :
  ![[Pasted image 20240408115525.png]]
  These are all the supported frequencies by your wifi device.

- Lets say now, you want to know the ==bit-rate== of your wifi device. In this case you run :
  
```
  iwlist wlo1 bitrate
```

![[Pasted image 20240408115753.png]]

- To display the ==Bandwidth== its another story, here you must use another command :
  
```
  iw dev wlo1 info
```
![[Pasted image 20240408120230.png]]


- More about `iw dev` :
  ![[Pasted image 20240408120310.png]]


# Changing the Wifi interface name

- To view the Informations about your Wifi Interface, run the command with `udervadm` :
  ![[Pasted image 20240418155737.png]]
- To edit the name of your Wifi Interface, you need to create a rule in the udev directory :
  
```bash
  sudo nano /etc/udev/rules.d/10-persistent-net.rules
```

And add the Folowing Line :

```bash
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="xx:xx:xx:xx:xx:xx", NAME="eth0"
```

And apply the changes with :

```bash
sudo udevadm control
sudo udervadm trigger
```
