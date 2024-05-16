

# 10.

- Answer is *B*.

![[Pasted image 20240426192035.png]]

# 12.

- Answer is *B*.
- Whitin systemd service configuration, you can use the command `ExecStop` , `ExecStart`, `ExecReload` to Start, Stop or reload the service.
  ![[Pasted image 20240426192608.png]]

# 20.

- Answer is *C*.
- Lets look at the `/etc/systemd/journald.conf` :
  ![[Pasted image 20240426202359.png]]


# 28.

- Answer *A*.

![[Pasted image 20240505124420.png]]

# 32.

- Answer is *D*.
- To display memory usage incl. swap, use the `free` command :
  ![[Pasted image 20240505124826.png]]

![[Pasted image 20240505124844.png]]


# 48.

- Answer is *C*.
- To display the Disk-Space related to the journalctl, run the command :
  
```bash
  journalctl --disk-usage
```

![[Pasted image 20240505130912.png]]

- More in [[Log File Administration]].


# 55.

- Answer is *D*.
- To test the connectivity of a web server for example `www.google.com` at port `443` , use the command : 
  
```bash
  openssl s_client -connect www.google.com:443

```

![[Pasted image 20240505132228.png]]
![[Pasted image 20240505132246.png]]

# 56.

- Answer is *A*.

# 63.

- Answer is *A*.
- To determine how much time a command takes on linux, you run the command :
  
```bash
  time <command>
```

![[Pasted image 20240505134224.png]]

# 64.

- Answer is *B*.
- Change the default gateway to `192.168.1.1` using `eth0` :
  
```bash
  ip route change default via 192.168.1.1 dev eth0
```

# 65.

- Answer is *C*.
- To display the Bad Blocks with `dump2fs` use the option `-b` :
![[Pasted image 20240505135200.png]]

# 67.

- Answer is *D*.
- option within a systemd mount file specifies the filesystem to be mounted is `What` :
  ![[Pasted image 20240505135640.png]]

# 69.

- Answer is *A*.
- To Add a rejecting route for an ip address :
  
```bash
  route add -­host 192.168.1.3 reject
```

- For a complete Network :
  ![[Pasted image 20240505141017.png]]


# 70.

- Answer is *B*.
- More in [[Troubleshooting the Network]].

# 78.

- Answer is *B*.
- The option within journald.conf sets a limit on how much disk space can be used is : `SystemMaxUse` 
![[Pasted image 20240505142300.png]]

# 81.

- Answer is *B*.
- To specify the type of the filesystem (nfs) in systemd use the option `Type=` .
- More in [[Log File Administration]].

# 87.

- Answer is *D*.
- `iostat -p` display all the Input/Output Statistics of all the Disks (Hardware) :
  ![[Pasted image 20240506115529.png]]

- Look at [[Performance Monitoring]].

# 93.

- Answer is *C*.
- The CPU is named *sy* in the top command :
  ![[Pasted image 20240506121400.png]]

# 95.

- Answer is *C*.
- `swapon --show` displays the informations about swap space along with the used space :
  ![[Pasted image 20240506121633.png]]

# 110.

- Answer is *B*.

- `fsck -r` : fsck report statistics.
  ![[Pasted image 20240506165144.png]]

# 123.

- Answer is *B*.

- Display the process ID of the a giving process (name) 
  
  
```bash
  pidof sshd
```

![[Pasted image 20240506171303.png]]

# 125.

- Answer is *A*.
![[Pasted image 20240506180052.png]]

# 138.

- Answer is *A*.
- To troubleshoot services with ports (TCP) use the command :
  
```
  tcptraceroute
```

![[Pasted image 20240506193614.png]]


# 142.

- Answer is *C*.
- Look at [[Working with Hard Disk Drives and SSDs]].

# 201.

- Answer is *B*.

![[Pasted image 20240510093600.png]]

# 221.

- Answer is *A*.
### Show parameters of a service

- To inverstigate a service run the command :
  
```bash
  sudo systemctl show <service>
```

- Lets take the example `ssh.service` :
  ![[Pasted image 20240512111306.png]]

# 231.

- Answer is *A*.
![[Pasted image 20240512120734.png]]

# 240.

- Answer is *B*.
![[Pasted image 20240512123631.png]]


