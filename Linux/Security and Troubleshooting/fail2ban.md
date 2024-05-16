- Fail2ban is a security tool used primarily on Linux servers to protect against unauthorized access attempts by monitoring logs for potentially malicious activity and taking action to block the offending IP addresses. Its main purpose is to prevent brute-force attacks on services such as SSH, FTP, HTTP, SMTP, and others.
- More about fail2ban in the [Arch Wiki](https://wiki.archlinux.org/title/fail2ban). Or in the Video `5. Given a scenario, implement and configure Linux firewalls.mp4`

## Installation

- To install fail2ban, run the command :
  
```bash
  sudo apt install fail2ban
```

- To start the Service :
  
```bash
  sudo systemctl start fail2ban
```

## Monitoring the Logs

- The fail2ban uses the systemd service, you can each time for the monitored services inside the `journalctl` :
  
```bash
  sudo journalctl -u fail2ban
```
![[Pasted image 20240415130744.png]]

- Or you can manuelly read the log file inside the file :
  
```bash
  cat /var/log/fail2ban.log
```
![[Pasted image 20240415130711.png]]

## Configuring fail2ban

### fail2ban config file

- Go to the file `vim /etc/fail2ban/fail2ban.conf` and make the changes you want :
  ![[Pasted image 20240415132729.png]]

### Configuring jails

- To configure a new fail2ban monitoring serivce, you need to create a new file inside the directory `/etc/fail2ban.d/jail.d/` .
- Lets take an example with ==SSHD== :
  
```bash
## First create the file sshd.conf :

sudo vim /etc/fail2ban.d/jail.d/sshd.conf

## Second, add the configurations :

[sshd]  
enabled   = true  
filter    = sshd               ## service to filter
banaction = iptables           ## use iptables to block
backend   = systemd            ## runned by systemd
maxretry  = 3                  ## after 1 attempt
findtime  = 1d                 ## monitor for 1 day
bantime   = 2w                 ## block for 2 weeks
ignoreip  = 127.0.0.1/8

## Save, close and restart the service 
```

- Now restart the service :
  
```
  sudo systemctl restart fail2ban
```

### Check jail status

- To list enabled jails and their status :
  
```
  sudo fail2ban-client status
```
![[Pasted image 20240415131427.png]]

- Show a detailed status of a specific jail :
  
```
  sudo fail2ban-client status sshd
```
![[Pasted image 20240415131539.png]]

