- To change the Hostname of your Machine, you can use the command `hostname <new_hostname>` or edit the file `nano /etc/hostname`.
- Many Devices detect each others with the implementation of the Hostnames. Each Machine has a Hostname corresponding to his IP-Address.
- Hostnames and there IP-Addresses are stored locally in the `/etc/hosts` file :
  ![[Pasted image 20240304125708.png]]

- Because its very difficult to assign every IP-Address on the Internet in the `/etc/hosts`, the Linux system asks the DNS server for associated IP-Address of a certain Domain or User.
- The DNS servers are located in the `/etc/resolv.conf` file:
  ![[Pasted image 20240304130118.png]]
  - The IP-Address is the router, where the DNS request has to be sent.
  - or :
    ![[Pasted image 20240304130231.png]]

- In the image above, they are 3 DNS Servers. if one of them is unavailable, the system calls the second DNS Server ...
- The *Systemd-resolved* service handles name resolution requests, each DNS request is sent to the special address `127.0.0.53` loop back address :
  ![[Pasted image 20240304130741.png]]

> [!Note]
> - To test the DNS configuration by resolving a hostname IP-Address you can use the commands : nslookup, `dig`, or `host`.

### Address Resolving Law

- The Law behind the IP Resolving is saved in the file `/etc/nsswitch.conf` :
  ![[Pasted image 20240304165243.png]]

- This configuration file shows the order in which addresses are resolved (for example `google.com`) :
  
  1. first database that is checked is the `file` or the `/etc/hosts` (resolving google.com with it, if its not listed, do next)
  2. second is `mdns4_minimal` . `mDNS` is a protocol that allows devices on a local network to resolve each other's hostnames without the need for a centralized DNS server.If the system doesn't find `google.com` using mDNS, it will immediately return without proceeding to the next step, as indicated by `[NOTFOUND=return]`.
  3. When the system fails to resolve the address, it works with the third variable which is the default `dns`, which is the default dns server (8.8.8.8).
  4. When all of this fails, the system looks in the `myhostname` or in the `/etc/hosts` to resolve the address :
     ![[Pasted image 20240305124721.png]]
