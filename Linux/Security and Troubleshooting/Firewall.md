## firewalld

- More in the Arch [Wiki](https://wiki.archlinux.org/title/firewalld).
- First ensure that you installed the firewalld :
  
```
  sudo apt-get install firewalld
```

- The *Network Zones* are implemented in firewalld to maintain different sets of firewalls for your environment.
- Common network zones :
  ![[Pasted image 20240316151240.png]]

- The default Network Zone is defined in the `/etc/firewalld/firewalld.conf` :
  ![[Pasted image 20240316151524.png]]
   and the custom Network Zone is defined in the `/etc/firewalld/zones` directory.

- To manage your firewall use the command :
  
```
  firewall-cmd
```
![[Pasted image 20240316151802.png]]

![[Pasted image 20240316152243.png]]


- To display which zone is been currently used, use the command :
  
```
  firewall-cmd --get-active-zone
```

- To set the default zone (permanently) :

```bash
firewall-cmd --set-default-zone=internal
```
## Public

### Block/Allow Ports 


- More in [Firewalld](https://docs.e2enetworks.com/security/firewall/firewalld.html)
- Lets say you are on a *public* zone and you want to block these port and add them to the public block lists :

```bash
111/tcp  open  rpcbind
2022/tcp open  down
2049/tcp open  nfs
4445/tcp open  upnotifyp
```

- You block the ports in your zone, with the option `--remove-port=number/type`.  
- `--permanent` so that even when you restart your device, the changes will no be lost :
  
```bash
firewall-cmd --zone=public --remove-port=111/tcp --permanent
firewall-cmd --zone=public --remove-port=2022/tcp --permanent
firewall-cmd --zone=public --remove-port=2049/tcp --permanent
firewall-cmd --zone=public --remove-port=4445/tcp --permanent

```

- And then reload the firewall configurations :
  
```bash
firewall-cmd --reload
```

- Add a port to the Open ports that are allowed in the zone :
  
```bash
  sudo firewall-cmd --add-port=514/tcp --perment
```


> [!Note]
> 
> - If you get this Error :
>   ![[Pasted image 20240316210820.png]]
>   
>   Try and remove the Service and not the Port :
>   ![[Pasted image 20240316210911.png]]


### Block ping


- To block any ping from the Public WiFi, add this instruction to the firewalld :
  
```bash
sudo firewall-cmd --zone=public --add-icmp-block=echo-reply --permanent
sudo firewall-cmd --zone=public --add-icmp-block=echo-request --permanent

## and reload

sudo firewall-cmd --reload
```


## Internal

- Lets took a look at the parameters of the zone *internal* :
  ![[Pasted image 20240316213229.png]]

### internal trusts only the Internal Network

- To do so we need to made some changes to each of the parameters :
  
- `Interfaces :` You can add the interface, to which this changes gonna apply :
  
```bash
sudo firewall-cmd --zone=internal --add-interface=wlan0 --permanent
```

- `Sources :` Enter the allowed IP-range with the `--add-source` :

```bash
sudo firewall-cmd --zone=internal --add source=192.168.1.100 --permanent
```

- `Services :` add or Remove services with `--add-service` or `--remove-service` :

```bash
sudo firewall-cmd --zone=internal --add-service=ftp --permanent
```

- `Ports` add or remove Ports with `--add-port` or `--remove-port` :

```bash
sudo firewall-cmd --zone=internal --add-port=8080/tcp --permanent
```

- `Protocols :` add a protocol with `--add-protocol` :

```bash
sudo firewall-cmd --zone=internal --add-protocol=icmp --permanent
```

- `Forwarding :` To forward a port, lets say from port 80 to 8080 :
  
```bash
sudo firewall-cmd --zone=internal --add-forward-port=port=80:proto=tcp:toport=8080 --permanent 
```

- Finally, reload the Firewall :

```
sudo firewalld-cmd --reload
```

- Final Results :
  ![[Pasted image 20240316215958.png]]

==This setting blocks the ports from being displayed from another network, and are open only for the internal network !==


# Firewall GUI

- To install the Firewalld in a gui form, install the `firewall-config` :
  
```
  sudo apt-get install firewall-config
```

- It looks like this :
  ![[Pasted image 20240317124045.png]]
