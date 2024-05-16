
# Theory

- *Iptables* is a build in kernel module of the linux system. it utilizes the *netfilter* kernel component.
- More in this Website [iptables](https://www.booleanworld.com/depth-guide-iptables-linux-firewall/)
- Install the iptables with the command :

```
sudo apt install iptables  
sudo apt-get install iptables-persistent
```

![[Pasted image 20240318102358.png]]

- When a packet gets into the network interface, this packet would go to different set of rules. First the packet goes into one of this *Chains* (PREROUTING,INPUT,OUPUT,FORWARD,POSTROUTING), inside of each chain is a *table* (raw,mangle,nat,filter), which also contains a set of rules called *Target* which at the end decides what to do with a packet (ACCEPT,DROP, REJECT(connection timed out)).

![[Pasted image 20240420120316.png]]

#### Chains

- Allows you to filter the packets.
  
  - *PREROUTING* : apply to the packets that just arrived to the network interface. This chain is represented by the raw,mangle and nat table.
  - *INPUT* : apply to packets just before to arrive to a particular process. Its represented by mangle,filter.
  - *OUTPUT* : apply to packets just after they have been processed by a process. Its represented by raw,mangle,filter.
  - *FORWARD* : apply to packets that are routed through the current host to different one (mangle,filter).
  - *POSTROUTING* : apply to packets that are leaving the network interface (nat, mangle).

#### Tables

- Tables allows you to do specific things with a packet :
  
  - *filter* : make a decision if the packet should arrive at the destination.
  - *mangle* : change the packet values (TTL).
  - *nat* : route the packet to different hosts on the NAT, by changing the source and destination addresses of the packets >> allow services that cant be accessed by one computer.
  - *raw* : inspects the state of packets before it get processed.

#### Targets

- Determine what to do with a packets, when it matches a specific rule. Here where the fate decision has to be made. These option comes after the option `-j` :

  - *ACCEPT* : accept the packet.
  - *DROP* : drops the packet. silently discard packets without sending any response to the sender.
  - *REJECT* : Rejects the packet by sending a response back to the sender.
  - *LOG* : write the log informations about the packet in the system log
  - *DNAT* : Perform a *destination* NAT, modifying the Destination address of a packet.
  - *SNAT* : Perform a *source* NAT, modifying the Source address of a packet.
  - *MASQUERADE* : similar to SNAT, but used to dynamic IP Addresses.
  - *REDIRECT* : Redirect the packet to a local socket.
  
 
## Blocking IPs

- Lets say you want to block the IP-Address `59.45.175.62`. What this means is, you need to filter all kind of packets that arrive at your process and block them. This translates in :
  
	```bash
iptables -t filter -A INPUT -s 59.45.175.62 -j REJECT
	```

	- `filter` : filter mode
	- `INPUT` : because, this packet is about to enter the process.
	- `REJECT` : rejects the packet and send the response back to the sender.
	- `-A` : append to the existing list.


> [!NOTE]
> - The filter method is used by default.
> - You can also block a whole Network :
>   `iptables -A INPUT -s 59.45.175.0/24 -j REJECT`


- If you want to block the packets destinated to a specific IP-Address from your process, you can use the command with `-A OUTPUT` :
  
```bash
  iptables -A OUTPUT -d 31.13.78.35 -j DROP
```

## Deleting rules

- Lets say you want to delete the rule you made :
  
```
    iptables -A OUTPUT -d 31.13.78.35 -j DROP
```

- Now replace the `-A` with `-D` :

```
  iptables -D OUTPUT -d 31.13.78.35 -j DROP
```


## Inserting and replacing rules

- Imagine you have been attacked from the Network 59.45.175.0/24, so you want to blacklist them, but allow only one IP-Address 59.45.175.10.
- You first add the blacklisted Network, and right after that you need to add a rule to accept the packets from the IP-Address 59.45.175.10 :
- This will append the rule to the same chain :

```
  iptables -I INPUT 1 -s 59.45.175.10 -j ACCEPT
```

![[Pasted image 20240318113114.png]]

- Lets say you have, by mistake entered the wrong IP-Address. Lets you wanted to whitelist the IP 59.45.175.14 instead of the IP 59.45.175.10. In this case you can add the `-R` option :
  
```
  iptables -R INPUT 1 -s 59.45.175.14 -j ACCEPT
```

![[Pasted image 20240318113400.png]]


## Protocols and modules


- Lets say you want to block all incoming TCP traffic. you can do that by adding the option `-p` to specify the protocol of the packet to match :
  
```
  iptables -A INPUT -p tcp -j DROP
```

- You can also do that to `icmp` and `udp` .
- Lets say you want to block any access to the ssh port from a giving Network. To do that you need to filter the packet in hand of the destination port (port 22). to do that more accurately you need to load the module tcp with the flag `-m tcp` to analyse the packet in hand of the destination port. `-p tcp` only looks for tcp packets and dont look deeper into the port, that the packet is targeting.
  
```bash
iptables -A INPUT -p tcp -m tcp --dport 22 -s 59.45.175.0/24 -j DROP
```

### block more that one port

- If you want to block more that one port from been accessed from a particular network, you need to add the option `-m multiport`. 
- Lets say you want to block, both the port `22` and `5901` :
  
```bash
iptables -A INPUT -p tcp -m multiport --dports 22,5901 -s 59.45.175.0/24 -j DROP
```


## The connection tracking module

- If you block a certain IP Addresses, you will notice that you cant access this IP Addresses neither ! its like the INPUT chain affects the OUTPUT. 
- To overcome this issue, you need to tell the iptable to not touch the packets that are part of an existing connection.
- Iptables provides a module for that and its named `-m conntrack`.
- ==Connections tracked== have the following states :
  
  - *NEW* : represents the very first packet of a connection.
  - *ESTABLISHED* : packets that are part of an existing connection.
  - *RELATED* : packets that are related to an established connection. like ssh, where the connection was established, and the packets that get transferred are part of an preexisting one
  - *DNAT* : packets whose destination was changed by the rules in the nat table.
  - *SNAT* : like DNAT, this statement represents packets whose source addresses was changed,
  - *INVALID* : packets that doesn't have a propre state.
- To use this module, you need the add the state after the option `--cstate` .

```bash
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
```

- Droping the INVALID packets is also a good idea :
  
```bash
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```


## Changing the default policy

- You can change the default chain policy from ACCEPT to DROP.
- The default chain policy :
![[Pasted image 20240318120547.png]]

- To change the default chain policy of the Chain INPUT from ACCEPT to DROP :

```bash
iptables -P INPUT DROP
```

## Selecting interfaces

- To allow the services to communicate localy between each other (Apache,nginx), which succeed localy through the loop-back address `lo` :
  
```bash
  iptables -A INPUT -i lo -j ACCEPT
```

- To drop any communication (packets get sent to the Network) between the local machine and the Network `121.18.238.0/29` through the interface `wlan0` (Packets are sent out through wlan0, that why `-0` for output):

```
iptables -A OUTPUT -o wlan0 -d 121.18.238.0/29 -j DROP
```
 

## Negating conditions (Only a giving ports are open)

- To isolate your Server in such a manner that only giving port are allowed to be open, you can use the negation condition `!`.
- To only accept the TCP traffics that are destinated for HTTP,HTTPS and SSH are allowed to be open :
  
```bash
iptables -A INPUT -p tcp -m multiport ! --dports 22,80,443 -j DROP
```

## iptable commands


- To Delete (*Flush*) all the changes use the command :
  
```
  sudo iptables -F
```

- To *list* all the iptables rules in a verbose way use the command :
  
```
  sudo iptables --nL --line-numbers
```
![[Pasted image 20240317181847.png]]

- To remove the iptable rule, ==in general== select the chain number and enter `sudo iptables -D chain rule_line_number`. in this example : `iptables -D INPUT 1` 
  ![[Pasted image 20240317182005.png]]

- Iptables options :
  ![[Pasted image 20240316145122.png]]

- To ==save== your configurations before rebooting, use the command:
  
```
  iptables-save > /etc/iptables/rules.v4
```

- To ==Restore== your previous configuration enter the command :
  
```
  iptables-restore < /etc/iptables/rules.v4
```

- To ==List== the table name use the option `-t`.

![[Pasted image 20240421131013.png]]
### Removing the comfigurations

- To remove the configuration, run the command :

```
sudo iptables -t nat -L --line-numbers  
```
![[Pasted image 20240317184637.png]]

- Select the number and delete it with :
  
```
sudo iptables -t nat -D PREROUTING <rule_number>
```


# Exercises


> [!Exercise 1]
> - Set up a default firewall policy that allows all outgoing traffic (`OUTPUT` chain) and blocks all incoming traffic (`INPUT` chain).
> - Add rules to allow SSH (port 22) and HTTP (port 80) traffic to your system while blocking all other incoming traffic.
> - Test the firewall configuration by attempting to connect to your system via SSH and HTTP from another device.

## Solution 

- Allowing all outgoing traffic :
  
```
  iptables -A OUPUT -j ACCEPT
```

- Droping all incoming traffic :

```
 iptables -A INPUT -j DROP
```

- Allowing only SSH and HTTP to the device (input):

```
iptables -A INPUT -m multiport -p tcp ! --dports 22,8  
0 -j ACCEPT
```


> [!Exercise 2 : Port Redirection]
> - Configure port forwarding using `iptables` to redirect traffic from one port to another (localy). For example, redirect traffic from port 8080 to port 80.
> - Test the port redirection by accessing a service on the redirected port and verifying that it works as expected.



### Tables

- The 2 most used tables in the routing chain are :
  
  - **filter :** It is the default table, it filters the packets packets for the local machine and its chains (if `-t` is not specified ), it contains the *INPUT*, *FORWARD*, *OUTPUT*.
  - **nat :**  this table is consulted, when a new connection is encountered. It consists of 4 chains :
    
    - *PREROUTING :* altering the packets as soon as they come (Forwards the traffic to another Network/Device on another port) :

	```bash
      iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to-destination 192.168.1.20:80
	```
	
    - *INPUT :* altering sockets that are destined to the local host. This changes are made on the local host.
	
	```bash
	iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j DNAT --to-destination 192.168.1.20:443
	
	iptables -A INPUT -i eth0 -p tcp --dport 443 -j ACCEPT
	```
    
    
    - *OUTPUT :* altering the localy generated packets.

	```bash
	iptables -t nat -A OUTPUT -p tcp --dport 443 -j DNAT --to-destination 192.168.1.40:443
	```


    - *POSTROUTING :* altering packets as soon as they are about to go (like translate the old IP Address to a new one)

	```bash
	iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j SNAT --to-source 203.0.113.10
	```

![[Pasted image 20240316130134.png]]


## nat

- *nat* is used to manipulate the packets as they travel through different networks and ports.

### NAT

- Look for [[2 Bypassing Network Acces Control]].

> [!Important]
> - NAT is used to modify the Destination of IP Addresses and/or port of incoming packets 

- Here is an example of a NAT Network :
  
  - Example :
    
```bash
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j DNAT --to-destination 192.168.1.10:443
```

  - Explanation :
    
    This command forwards the traffic from an incoming HTTPS on the interface eth0, from an external Network to an internal Web Server (IP 192.168.1.10) on port 443.
    - `-j DNAT` : Destination NAT.
    - `--dport 443` : destination port.


### SNAT

- Here is an example of a SNAT Network :
  
  - Command :
    
```bash
iptables -t nat -A POSTROUTING -s <source_network> -o <out_interface> -j SNAT --to-source <public_ip>`
```

  - Example :
    
```bash
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j SNAT --to-source 203.0.113.10
```

  - Explanation :
    
   This Command sets up the Source NAT (SNAT) to translate the traffic that comes from the `192.168.1.0/24` on the interface `eth0` to the IP Address `203.0.113.10`

### DNAT

- Here is an example of a DNAT Network :
  
  - Command :
    
```bash
iptables -t nat -A PREROUTING -i <in_interface> -p <protocol> --dport <destination_port> -j DNAT --to-destination <internal_ip>:<internal_port>
```

  - Example :
    
```bash
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to-destination 192.168.1.100:8080
```

  - Explanation :
    
    This command forwards The TCP traffic on 192.168.1.100 from port 80 to port 8080.


### Port Forwarding

- The Port Forwarding works in the same way as the DNAT !

### Removing the comfigurations

- To remove the configuration, run the command :

```
sudo iptables -t nat -L --line-numbers  
```
![[Pasted image 20240317184637.png]]

- Select the number and delete it with :
  
```
sudo iptables -t nat -D PREROUTING <rule_number>
```

## filter

### INPUT

- Allow SSH on the local machine :
  
```
  iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

- Allow HTTP (80) on the local machine :

```
 iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

- Allow ICMP (ping) :
  
```
 iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```

- Drop all incoming traffic except established connections :

```
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -j DROP
```

### OUTPUT

- Allow outgoing DNS (53) :

```
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
iptables -A OUTPUT -p tcp --dport 53 -j ACCEPT
```

- Allow outgoing traffic to a specific IP :

```
iptables -A OUPUT -d 203.0.113.10 -j ACCEPT
```

### FORWARD

- Forward HTTP traffic to an internal server :

```
iptables -A FORWARD -i eth0 -p tcp --dport 80 -d 192.168.1.10 -j ACCEPT
```

- Drop all the packets that are coming from the internal network to external :

```
iptables -A FORWARD -s 192.168.1.0/24 -o etho -j DROP
```