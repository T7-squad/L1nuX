- To display the Routing table of a specific Network you can use the command `route`, or `route -n` to display only IP Addresses.
  ![[Pasted image 20240305125356.png]]
  
  - The aliases can be find in the `/etc/networks` :
    ![[Pasted image 20240305125546.png]]
- Or you can use the command `ip route` :
  ![[Pasted image 20240305125411.png]]

- When your machine has more than one network interface, you need to forward your traffic from one interface to another. This is called *routing* or *IP forwarding*.
- To enable IP forwarding you need to add the number 1 to `/proc/sys/net/ipv4/ip_forward` for IPv4 Addresses and `/proc/sys/net/ipv6/conf/all/forwarding` for IPv6 Addresses :
  
  - *IPv4* : `echo 1 > /proc/sys/net/ipv4/ip_forward`.
  - *IPv6* : `echo 1 > /proc/sys/net/ipv6/conf/all/forwarding`.

- To enable the IPv4 forwarding at every boot, ensure that the line `net.ipv4.ip_forward = 1` exists in the `/etc/sysctl.conf` or withing a file under `/etc/sysctl.d`.
  ![[Pasted image 20240305130607.png]]
  
## How to route the Traffic ?

- Lets take this example :
  ![[Pasted image 20240305130712.png]]

- The Router A need The route the Traffic from the `1.0.0.0/8` Network to the `2.0.0.0/8` Network.
- If the Router A receives packets from the Network `1.0.0.0/8` network that is destinated to the `3.0.0.0/8` network, it needs to forward the packet to the Network `2.0.0.0/8` network through its default gateway which is `2.0.0.2`.
- To do that you need to add the `3.0.0.0/8` network to the routing table of the *router A* :
  
  - `route add -net 3.0.0.0 netmask 255.0.0.0 gw 2.0.0.2`.
  - OR `ip route add 3.0.0.0/8 via 2.0.0.2`.

- For *router B* :
  
  - `route add -net 1.0.0.0 netmask 255.0.0.0 gw 2.0.0.1`
  - OR `ip route add 1.0.0.0/8 via 2.0.0.1`.

- To ==delete== a Routing table use the command `route del` or `ip route del` to delete entries from the IP table.
- To Add a ==rejecting== route for an ip address :
  
```bash
  route add -­host 192.168.1.3 reject
```

- For a complete Network :
  ![[Pasted image 20240505141017.png]]

- ==Change== the default gateway to `192.168.1.1` using `eth0` :
  
```bash
  ip route change default via 192.168.1.1 dev eth0
```

- To ==pereserve the changes== run the command :
  
```bash
  ip route flush cache
```

- `route` :
  ![[Pasted image 20240305131708.png]]

- `ip route` :
  ![[Pasted image 20240305131743.png]]

- Network changes are often deleted when booting the system. You enable the changed also when you booting your system you need to add the entries to `/etc/netplan/01-network-manager-all.yaml` :
  
```bash
# For Router A
# 
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s3:
      dhcp4: true
      routes:
      - to: 3.0.0.0/8
        via: 2.0.0.1
```

- Then you need to configure the changes with the command `sudo netplan apply`.


## Troubleshoot the Network

- If your Computer is unable to connect to the network, the problem is likely routing related.
- To troubleshoot the routing problems, use the command `traceroute` :
  ![[Pasted image 20240305135216.png]]


- To troubleshoot services with ports (TCP) use the command :
  
```
  tcptraceroute
```

![[Pasted image 20240506193614.png]]

