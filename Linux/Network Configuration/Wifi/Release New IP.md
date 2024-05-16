
# static DHCP assignment

- If your Wifi doesnt respond to the `dhclient` and `dhclient -r` it probably means that the wifi has a static DHCP assignment, that means that the Wifi only relase the IP Address based on the MAC-Address.
## Change MAC

- Install the following package :
  
```bash
  sudo apt install macchanger
```

- Shut you interface down :
  
```bash
  sudo ip link set wlo1 down
```

- Change your MAC-Address with :
  
```
  sudo macchanger --random wlo1
```

- Check your MAC-Address with :
  
```
  sudo macchanger --show wlo1
```

![[Pasted image 20240506125130.png]]

- Now set your WLan up with :
  
```bash
  sudo ip link set wlo1 up
```

- Connect to the Wifi. You will get a new IP Address.
- 

