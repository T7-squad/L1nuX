# Securing the Local Computer

## Limiting Physical Access

- Configuring the PC to *NOT* boot up from USB-Device.
- Ensure that your BIOS is Password Protected.
- Linux and Widows uses the command `Ctrl+Alt+Del` to boot up / Shutdown the System. 

## Limiting Access to the operating system

### Password Management (lock/unlock)

- To prevent the users from using a weak password, you can configure the *Pluggable Authentication Module PAM*.
- The content of the `/etc/pam.d` :
  ![[Pasted image 20240314124340.png]]
- On Debian based systems you can change the following config file : `vim /etc/pam.d/common-password` :
  ![[Pasted image 20240314123413.png]]
  
  - You can add this variables : `password requisite pam_cracklib.so minlen=10 dcredit=1 ucredit=1 lcredit=1 difok=4` :
    - `minlen=10` : minimum of 10 Characters.
    - `dcredit=1` : at least one character.
    - `ucredit=1` : at least one uppercase.
    - `lcredit=1` : at least one lowercase.
    - `difok=4`   : for characters that are different from the previous password.

- To use the PAM to enforce the password complexity using `pam_cracklib.so`, you add this line in each place you want it to be inside the `/etc/pam.d` (ssh,login...) :
  
  ```bash
	auth required pam_tally2.so deny=3 unlock_time=300
    ```

    - `unlock_time=300` : lock 300 seconds after the last unsuccessful login.
    - `deny=3` : lock after 3 attempts.

- Or you can use the `pam_faillock.so`, this will *lock* the user after 3 attempts within 300 seconds and will be unable to lock into the user session until their account has been unlocked :
  
  ```
	auth required pam_faillock.so authfail deny=3 unlock_time=300
    ```
    
    - If another used has been locked, you can use the command `faillock --user user --reset` to reset the lock  :
      ![[Pasted image 20240314130457.png]]

### Kerberos

- *Kerberos* can also be used to manage Password withing an *LDAP*.
- After a user receives a *Kerberos ticket* he can authenticate to the AD.
- This method is called *SSO (single sign-on)* : If bob connects to an AD as the user bob.burt@example.com, the AD will identify him as bob.burt each time he reconnect to the AD.
- The *sssd (System Security Services Daemon)* on linux provides for LDAP and Kerberos connectivity and *Realm Daemon (realmd)* is used to discover and join the Active Directory domain :
    ![[Pasted image 20240314131307.png]]
    
    - To install Kerberos on Linux :
      
```
      apt install krb5-user heimdal-clients realmd
```

- To join an Active Directory, you can use the command `realm join example.com -v` :
  ![[Pasted image 20240314131840.png]]

- After login into the Active Directory you can use the command `klist` to view the Kerberos ticket informations


## Using GNU Privacy Guard (GPG)

- GPG is used to encrypt files.
  ![[Pasted image 20240314145412.png]]
  
- To use GPG, you need to create a GPG public/private Key with `gpg --gen-key` :
  ![[Pasted image 20240314145450.png]]

- The gpg key generator, generates a password that encrypt/decrypt files. this password is stored in the directory `~/.gnupg` along side with the GPG settings.


> [!Important]
> - The Key is stored as `pubring.gpg` in `~/.gnupg`
>   ![[Pasted image 20240419133159.png]]

### Send the Key

- To send the key to a key server :
  
```bash
  gpg --keyserver <key-server> --send-keys <key-id>
```

- `<key-server>` is the address of the key server where you want to send the keys.
- `<key-id>` is the ID or fingerprint of the key you want to send.

For example, if you want to send a key with ID `ABC123` to the key server `pgp.mit.edu`, the command would be:

```bash
gpg --keyserver pgp.mit.edu --send-keys ABC123
```

### Encrypt

- To encrypt a file use the command : `gpg --encrypt --sign -r` :
  ![[Pasted image 20240314150050.png]]
  
### Decrypt

- To decrypt a file, use the command `gpg` along side with the file : `gpg topsecretdata.gpg` :
  ![[Pasted image 20240314151436.png]]


## Practicing Secure Linux Administration


  - Ensure that the users doesn't have any unnecessary File and Directory permissions.
  - Restricting the users who are allowed to schedule command on crontab.
  - Viewing the history of successful login attempts using :
    - `who`.
      ![[Pasted image 20240314153612.png]]
    - `strings /var/log/wtmp` :
      ![[Pasted image 20240314154044.png]]

    - `last` 
      ![[Pasted image 20240314160827.png]]
      ![[Pasted image 20240314154120.png]]
    - `lastb`


## Using Encryption

### FTP

- To ensure the security of your services, like for example FTP, you can use the encrypted FTPS (FTP + SSL/TLS) instead of the unencrypted FTP.
- You should force any connection that uses FTP or SFTP to use FTPS.
- Install the *FTPS* : `sudo apt install vsftpd`.
- You can do that by going to `/etc/vsftpd.conf` adding the 2 lines :
   
```bash
ssl_enable=YES
force_local_logins_ssl=YES
```

![[Pasted image 20240314162750.png]]

- This will force FTP to use SSL/TLS certificate.

## Limiting System Access for Network Services


- Make sure that daemons like Apache or Postgresql does not run a shell in the `/etc/passwd`. Because if a hacker compromises the SQL database, he can run a shell inside the machine :
  ![[Pasted image 20240314203523.png]]
  
  - Lets change the shell of PostgreSQL to a non interactive shell like `/sbin/nologin`. To do that run the command `chsh -s /usr/sbin/nologin postgres` :
    ![[Pasted image 20240314203744.png]]

 - Now you should restart your PostgreSQL :
   
```
   systemctl restart postgresql
```


### Prevent a user from login in 

- To prevent a user from login into the system, you can use the command `chsh -s /usr/sbin/nologin hitman` :
  ![[Pasted image 20240314204812.png]]

- This will prevent the user from having a shell.
- Lets prevent the user `hitman` from login in his shell :
  ![[Pasted image 20240314205059.png]]

### hosts.allow and hosts.deny

#### hosts.allow

- `hosts.allow` will allow a particular (or ALL) users to connect to a specific (or ALL) Services :
1. *Allow ALL to connect to ALL* :
   
```
   ALL:ALL
```

2. *Allow a particular user/Network to use all the services :*

```bash
# IP 1.2.3.4 freigeben:
ALL: 1.2.3.4

# Alle Adressen im Netz 4.5.6.x freigeben (Netzmaske 4.5.6.0/24)
# Jede der Zeilen ist eine mögliche Eingabeform und bewirken das gleiche:
ALL: 4.5.6.
ALL: 4.5.6.0/24
ALL: 4.5.6.0/255.255.255.0
```

3. *Allow a particular user/Network to use a particular services :*

```bash

## FTPS 

vsftpd: 192.168.1.0/24 
vsftpd: 10.0.0.5

## SSH

sshd: 192.168.1.0/24 
sshd: 10.0.0.5

```


> [!NOTE]
> - You can also Allow domains like `.linux.org` `.bmw.org`...
>   
> ```
>   sshd : .linux.org .bmw.org 
> ```
> 


#### hosts.deny

- `hosts.deny` is the all way around :
  
```bash

## Deny all except :

sshd: ALL EXCEPT 192.168.1.0/24



## Or, deny All ! :

sshd: ALL

## denies access to all services (`ALL`) from all hosts (`ALL`), except for the host with the IP address 192.168.1.10 :


ALL: ALL EXCEPT 192.168.1.10

```

- Options to `hosts.deny` :
	- Single IP address (Rejects the connection attempt with an ICMP response):
	
	- `REJECT: 192.168.1.100`
	    
	- IP address with subnet mask (Similar to `ALL`, blocks all connection attempts):
	    
	    
	- `DENY: 192.168.0.0/24`
	    
	- IP address with subnet mask in CIDR notation (Rejects the connection attempt with an ICMP response):
	    
	    
	- `REJECT: 10.0.0.0/255.255.255.0`
	    
	- Hostname (Treats the connection attempt as an error, potentially logging it):
	
	`ERROR: example.com`
## Using Secure File Permissions

- Lets say you are using the Apache daemon. When installing Apache the `/var/www/html` is then located in the root directory and has the permissions of the root user ( by default ) :
  ![[Pasted image 20240314215935.png]]
  
-  Which means that is if a hacker gained access to the Apache server, he can be able to modify the `index.html` file.
- To prevent this you need to set a group for the Web users that handles the Apache server :
  ![[Pasted image 20240314220152.png]]
- To do that :
  
  - Add the group `apache` : `sudo groupadd webdev`.
  - Add the user `webdev` and add it to the group `apache` : `sudo useradd -g apache -s /sbin/nologin webdev`.
  - Now change the ownership of the `index.html` : `chown webdev:apache /var/www/html/index.html`
    ![[Pasted image 20240314221024.png]]
