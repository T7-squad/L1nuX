- The Network device drivers are provided by the modules that are loaded by the kernel at boot time. To view this modules, that are responsible for the Network interfaces, run the command `sudo lshw -class network` :
  ![[Pasted image 20240304113440.png]]

- Or run `lspci -v` , to display the network interface hardware informations :
  ![[Pasted image 20240304113623.png]]

- First you need to install the `net-tools`, which gives you the access to many linux network tolls : `sudo apt-get install net-tools -y`.
- To display informations about a network run the command `ifconfig` :
  ![[Pasted image 20240304113813.png]]

- Or run the command `ip` :
  ![[Pasted image 20240304113845.png]]

- You can receive IP configuration from a DHCP provider, run the command `sudo dhclient >interface<` :
  ![[Pasted image 20240304114127.png]]

- You can also use the command `netstat` :
  ![[Pasted image 20240304114840.png]]

- To completely set a Network interface with ip addresses, you can also add entries in the `/etc/network/interfaces`, save the changes and reloading the interfaces with `ifconfig wlan0 down` and then `ifconfig wlan0 up`. After that you need to restart your `NetworkManager` with `sudo systemctl start NetworkManger`.
- Pre configured Network Interfaces are stored in the `/etc/NetworkManager/system-connections` directory (**with there passwords**) :
  
```bash
[root@server1 ~]# cd /etc/NetworkManager/system-connections
[root@localhost system-connections]# cat Wired1.nmconnection

[connection]

id=Wired1
uuid=e4006c02-9479-340a-b065-01a328727a75
type=ethernet
interface-name=eth0

[ipv4]

address1=3.4.5.6/8,3.0.0.1
dns=8.8.8.8;1.1.1.1;
method=manual

[ipv6]

addr-gen-mode=stable-privacy
method=auto

[root@server1 ~]#_
```

- `method=auto` : IP is obtained automatically via DHCP (IPv6)
- `method=manual` : IPv4 obtained manually.
- `address1=3.4.5.6/8,3.0.0.1` : default Gateway.
- `dns=8.8.8.8;1.1.1.1` : DNS.


- After making the changes to the `Wired1.nmconnection` run the command `nmcli connection down Wired1` and `nmcli connection up Wired1`.
- A simple `*.yaml` configuration in the `/etc/netplan/00-installer-config.yaml` could be, for example :
  
  
```
[root@server1 ~]# cat /etc/netplan/00-installer-config.yaml
network:
	version: 2
	renderer: networkd
	ethernets:
		eth0:
			dhcp4: no
			dhcp6: yes
			addresses: [3.4.5.6/8]
			gateway4: 3.0.0.1
			nameservers:
				addresses: [8.8.8.8,1.1.1.1]
[root@server1 ~]#_
```

## Using Systemd-networkd

- You can use the `systemd-networkd` instead of the `NetworkManager`.

> [!Important]
> - If used, only one of them mus be active.

#### arp

- Here are some options to the `arp` command :
  
  ![[Pasted image 20240506130642.png]]

- The command :

```bash
  arp -s <IP_address> <MAC_address>
```

Creates a new entry for the giving IP and MAC Address.

#### nmcli

1. nmcli agent :

![[Pasted image 20240304122134.png]]

2. nmcli connection

![[Pasted image 20240304122220.png]]

3. nmcli monitor 

![[Pasted image 20240304122317.png]]

4. nmcli device

![[Pasted image 20240304122353.png]]

5. nmcli netwoking

![[Pasted image 20240304122429.png]]

6. nmcli radio

![[Pasted image 20240304122517.png]]

## iptraf

- iptraf-ng is an ncurses-based IP LAN monitor that generates various  network  statistics  including  TCP info, UDP counts, ICMP and OSPF information, Ethernet load info, node stats, IP checksum errors, and others.
- To install the tool :
  
```bash
  sudo apt install iptraf-ng
```

![[Pasted image 20240506182440.png]]
![[Pasted image 20240506182525.png]]
![[Pasted image 20240506182550.png]]





## Bandwidth test

- To test the Band-Width of your internet Connection, you can use the utility called `iftop`.
- Before that you need to install the tool :
  
```
  sudo apt-get install iftop -y
```

![[Pasted image 20240317131709.png]]



