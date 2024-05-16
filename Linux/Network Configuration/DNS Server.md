
# The `dig` Command

## Basic Record Types

- More Details about the ==Record Types== in This [Link](https://www.cyberciti.biz/faq/linux-unix-dig-command-examples-usage-syntax/).
- Check this also the [dnsdumpster.com](https://dnsdumpster.com/).
- [[Linux PrivESC TCM]]
- These are the basic Record Types :
  
1. **A (Address) Records**: Maps a domain name to an IPv4 address.
   
```bash
   dig example.com A
```
    
2. **AAAA (IPv6 Address) Records**: Maps a domain name to an IPv6 address.
   
```bash
dig example.com AAAA
```
    
3. **CNAME (Canonical Name) Records**: Alias of one domain name to another. It is often used to map multiple domain names to the same IP address.
   
```
   dig www.example.com CNAME
```
    
4. **MX (Mail Exchange) Records**: Specifies the mail servers responsible for receiving email messages for the domain.
   
```
 dig example.com MX  
```
    
5. **NS (Name Server) Records**: Specifies the authoritative nameservers for the domain.
   
```
   dig example.com NS
```
    
6. **PTR (Pointer) Records**: Performs reverse DNS lookups. It maps an IP address to a domain name.
   
```
   dig -x 192.0.2.1
```
    
7. **TXT (Text) Records**: Allows arbitrary text to be associated with a domain. It is often used for SPF (Sender Policy Framework), DKIM (DomainKeys Identified Mail), and other purposes.
   
```
   dig example.com TXT
```
    
8. **SOA (Start of Authority) Records**: Contains administrative information about the domain, such as the primary nameserver, the email address of the domain administrator, and various timing parameters.
   
```
   dig example.com SOA
```
    
9. **SRV (Service) Records**: Specifies the location of services such as SIP, LDAP, and XMPP in the domain.
   
```
   dig _sip._tcp.example.com SRV
```
    
10. **CAA (Certification Authority Authorization) Records**: Specifies which certificate authorities (CAs) are allowed to issue certificates for a domain. 
    
```
    dig example.com CAA
```

#### Basic DNS Query

- This command Query the IPv4 Address associated with a particular domain `google.com` :
  
```bash
  dig google.com
```

![[Pasted image 20240413193714.png]]

- The `IN` stands for the Internet.
- The Record Type `A` indicates an Address record (IPv4).

#### Tracing DNS

- To trace the DNS query through the DNS hierarchy run the command :
  
```bash
  dig +trace google.com
```

![[Pasted image 20240512112131.png]]



# The `nslookup` Command

- More In [[Passive Recon]].

![[Pasted image 20240413205837.png]]

# The `resolvectl` Command

[[Troubleshooting the Network]]

# The `whois` Command

- The `whois` command will give you more informations about an IP Address or an Address.
- Info :
  ![[Pasted image 20240415204418.png]]

- Here is an example with `whois arte.fr` :
  ![[Pasted image 20240415204806.png]]

- Or `whois 151.101.128.81`, The IP Address is of the BBC.com :
  ![[Pasted image 20240415204912.png]]
