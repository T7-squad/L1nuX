- The *Infrastructure Services* are : **DHCP, DNS, NTP**.

# DHCP

- When using the DHCP, the system sends a DHCP request to the Interface for an IP-Address.
- The DHCP make sure that two computer *DONT* have the same IP-Addresses.
- DHCP Server can also send the IP configurations such as DNS and default Gateway to the new User.

## DHCP Lease Process

![[Pasted image 20240307192004.png]]


## Configuring a DHCP Server

- To configure the DHCP on your linux machine you have to install the *dhcpd (DHCP daemon)*.
- Next, go to the config files to edit the IP-Address and other IP configurations :
  • `/etc/dhcp/dhcpd.conf` stores IPv4 configuration
  • `/etc/dhcp/dhcpd6.conf` stores IPv6 configuration
- An example of a `/etc/dhcp/dhcpd.conf` :

```bash
[root@server1 ~]# cat /etc/dhcp/dhcpd.conf
default-lease-time 36000;
option routers 192.168.1.254;
option domain-name-servers 192.168.1.200;
subnet 192.168.1.0 netmask 255.255.255.0 {
range 192.168.1.1 192.168.1.100;
}
[root@server1 ~]#_
```

- After making the changes to the `DHCP`, you need to restart the Service : `sudo systemctl restart dhcpd`.

# DNS

- More in [[Finding Subdomains]].
- DNS servers have resolve Names to IP Addresses -> *forward lookup*.
- DNS servers resolves IP Addresses to Names --> *reverse lookup* .

## The DNS Lookup Process

![[Pasted image 20240307193234.png]]

- If you want to browse the google website, your linux system sends a request to the DNS-Servers that are listed under the `/etc/resolv.conf` :
  ![[Pasted image 20240307195246.png]]
- If the your system can resolve the domain `google.com`, you get the response directly. If not, the request go to your ISP (`search ...`), if your ISP has the DNS record of google.com, you get the response back from him, if not your ISP sends the request to the Top Level DNS server, if its also fails the next hop is the DNS server zone (Photo above).
- All DNS Servers contain a *DNS cache file* that contains the IP Addresses of the DNS Servers that holds top-level DNS zones.

## Configuring a DNS Server

- To use your Linux Server as a DNS server, you need to install *DNS name daemon* `sudo apt-get install named`. and add resource records that list FQDN of computers in that zone as well as there IP-Addresses. 
![[Pasted image 20240307202447.png]]

- After that you need to restart the `named.service` : `sudo systemctl restart named.service`.
- You need also to restart the DNS same daemon with : `sudo systemctl restart bin9.service`.
- You can use the command `dig` to query the IP address of a specific DNS.

# NTP

- NTP (Network Time Protocol) is used to set the correct time for multiple or Single computers over the Internet, even if the hardware clock is modified !
- NTP is one of the oldest Internet Protocols, its designed to simplify the setting of time and date informations on computers across the internet using TCP/UDP on *Port 123*.
- The daemon responsible for that is the `ntpd` Service.
![[Pasted image 20240307203719.png]]

## Working with ntpd

- To configure your linux system as an NTP Server, you need to modify the `/etc/ntpsec/ntp.conf`.
- For example, the following lines in `/etc/ntpsec/ntp.conf` query three time servers using fast clock synchronization (`iburst`): `ntp.research.gov`, `ntp.redhat.com`, and `0.fedora.pool.ntp.org` :

```
pool ntp.research.gov iburst
pool ntp.redhat.com iburst
pool 0.fedora.pool.ntp.org iburst
```

- ![[Pasted image 20240307205818.png]]

- If your time differs significantly from the time on these time servers, you must first stop ntpd and then run the `ntpdate` command to manually synchronize the time. You might need to run the `ntpdate` command several times until the time difference (or `offset`) is far less than 1 second. After manually synchronizing the time in this way, you can start the ntpd daemon again. This process is shown in the following output:

```bash
[root@server1 ~]# systemctl stop ntpd.service
[root@server1 ~]# ntpdate -u 0.fedora.pool.ntp.org
4 Sep 15:03:43 ntpdate[2908]: adjust time server 206.248.190.142 offset
0.977783 sec
[root@server1 ~]# ntpdate -u 0.fedora.pool.ntp.org
4 Sep 15:03:53 ntpdate[2909]: adjust time server 206.248.190.142 offset
0.001751 sec
[root@server1 ~]# ntpdate -u 0.fedora.pool.ntp.org
4 Sep 15:04:01 ntpdate[2910]: adjust time server 206.248.190.142 offset
0.001291 sec
[root@server1 ~]# systemctl start ntpd.service
[root@server1 ~]#_
```

- To restart the ntpd daemon, you can use the command `ntpq` command to see what actual time servers you are synchronizing with :
  ![[Pasted image 20240307205940.png]]

- Example of the config file :
![[Pasted image 20240307210825.png]]
![[Pasted image 20240307210854.png]]

- More in [NTP_Youtube](https://www.youtube.com/watch?v=fBo0qQ_6mEI)
- If you want to allow other computers to query your NTP Daemon for time information, edit the `/etc/ntpsec/ntp.conf` and add the the specific computers or networks that are allowed to query Time from your NTP Server :
  
```
restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
```

- After that you need to restart your NTP Server.

## Working with chronyd

- To install *chrony* use the command : `sudo apt-get install chrony`.
- ![[Pasted image 20240307214158.png]]

- Modern linux distributions uses the *chronyd NTP* daemon to query time informations.
- You can add the NTP clients by adding the `allow subnet` to `/etc/chrony.conf`.
  - For example `allow 192.168.1.0/24` would allow clients of this network to query Time from the server.
- To view the servers that you are pulling for Time, run the command `chronyc source` :
  ![[Pasted image 20240307214123.png]]
