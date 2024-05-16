
- There a re a set of linux tools to scan for *rootkits*, *backdoors*, and possible *local exploits*.

# rkhunter

- `rkhunter` is a command line tool to scan for rootkits.
- Install it with the following command :
  
```
  sudo apt install rkhunter
```

![[Pasted image 20240423123309.png]]

- Lunch a scan with the `sudo rkhunter --scan` :
  ![[Pasted image 20240423123351.png]]
  ![[Pasted image 20240423123409.png]]
  ![[Pasted image 20240423123738.png]]
  ![[Pasted image 20240423123801.png]]

- For more ==detaled Informations== look for the logfile in :
  
```bash
  vim /var/log/rkhunter.log
```

![[Pasted image 20240423125021.png]]

# chkrootkit

- Another tool is the `chrootkit` :
  
```
  sudo apt install chrootkit
```

- Here are some options :

![[Pasted image 20240423125655.png]]

- Here is what the tool looks like :

![[Pasted image 20240423125621.png]]

