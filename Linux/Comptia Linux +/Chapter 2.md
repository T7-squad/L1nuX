# 4.

-Common BASH shell environment are typically executed in the order :
  
```
/etc/profile                 ## set the user (or root)
/etc/profile.d/*             ## set desktop enveronment 
/etc/bashrc                  ## alias for all users (System-wide!)
~/.bashrc                    ## alias for a specific user.
~/.bash_profile              ## set the variables, after login.
~/.bash_login
~/.profile
```

![[Pasted image 20240418131932.png]]

1. The shell looks for the `/etc/profile` and execute it .
2. The shell runs the `.bash_profile`. it basecally says to the shell to find the `.bashrc` file and execute it :
   ![[Pasted image 20240418130110.png]]

2. Execute the `.bash_login`. shell responsible for the login shells.
3. the `.profile` loads the private users variables and configurations.

# 6.

- The command `bash --norc` start a minimal interractive shell without loading a user specific configurations :
  ![[Pasted image 20240418133400.png]]

- This is how the shell looks like :
  ![[Pasted image 20240418133815.png]]
# 9.

- Answer : *B: X11Forwarding yes* 
  
![[Pasted image 20240418134819.png]]

- You muss enter the `X11Forward yes` on ther *Server* side. and `ForwardX11 yes` on the *Client* Side (`/etc/ssh/ssh_config`).
- Or type the command :
  
```bash
  ssh -o ForwardX11=yes username@server
```


# 9. 

- Answer : *D*
- Set a command on the username (add a real username) :
  
```bash
  sudo usermod -c "John Smith" john
```

- More in [[Administering Users and Groups]].

# 25.

- Answer : *D*
-  Lets brake down the root password in the `/etc/shadow` :
  
  `root:$6$PpnHJ3rl/6OouP9G$cKhZbi96b9UxfkHOt4ahHvrW9V73vg9BLqiEHKFOfcIAvktJ.fP2V9RoFkxgz6tcW9LlEspV2FN1j4g5vy90M1:17745:0:99999:7:::`


- The root, as many other users, passwords are stored in the `/etc/shadow` in this Form :
  
  `name:password:lastchange:min:max:warn:disable1:disable2:`
  
  - `min` : represents the minimum of days the user must wait until he change his password.*min=0 days*
  - `max` : represents the maximum of days the user can login using the same password. *max=99999 days* since since January 1, 1970.
  - `7` : you are warned 7 days before your password needs to be changed.

- More in [[Administering Users and Groups]].

# 28.

- To enable a *sticky bit* to a Directory (disabeling the deleting of a directory , like `/tmp` for example) you use the option `-t` :
  
```bash
  sudo chmod u+t /temp
```

# 29.

### `umask` 

- The Maximal number of permissions ( in general ) is `777`.
- To calculate the permissions with a `umask` rule of `177` for example is :
  - You muss first know it the file is *executable*. If it, you substart `7` from each number :
    
     - `7-1 = 6` : 6 gives the `rw` permissions.
     - `7-7 = 0` : gives no permissions.
     - `7-7 = 0` : gives also no permissions.

- For a *non executable* file, the umask would look like `022`.
  - The maximum number of permission is this example is : `666`. 
  - Lets find the permissions :
    
    - `6-0 = 6` : 6 gives `rw`.
    - `6-2 = 4` : 4 gives the `r` permission.


# 32.

- Answer : *D*.
- Here is the list of options for the `last` command :
  ![[Pasted image 20240418144903.png]]


# 36. 

- `openssl` option for creating a CSR certificate :
![[Pasted image 20240418145026.png]]
- More in [[Linux/Security and Troubleshooting/OpenSSL/Introduction]].

# 38.

- Answer : *B*.
- To view the Informations about your Wifi Interface, run the command with `udervadm` :
  ![[Pasted image 20240418155737.png]]

- To edit the name of your Wifi Interface, you need to create a rule in the udev directory :
  
```bash
  sudo nano /etc/udev/rules.d/10-persistent-net.rules
```

And add the Folowing Line :

```bash
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="xx:xx:xx:xx:xx:xx", NAME="eth0"
```

And apply the changes with :

```bash
sudo udevadm control
sudo udervadm trigger
```

- More in [[Wlan Interface]].


# 42.

- Answer is *C*.
- POSIX format is the `rwx` Format.
- The option is `-S` :
  ![[Pasted image 20240418161005.png]]

- Here how th output looks like :
  ![[Pasted image 20240418161230.png]]
- More in [[Permissions]].

# 46.

- The Answer is : *B*.
- The option `sudo chage -E -1` will remove the Expiration date :
  ![[Pasted image 20240418161638.png]]

# 47.

- Answer is *B*.
- You can use the command :
  
```
  getent
```

To Retrieve all the groups from (`/etc/nsswitch.conf`).
![[Pasted image 20240418162224.png]]


# 50.

- Answer is *A*.
- As you use your machine, the journalctl keeps track of your machine and stores the data in the `/var/log/journal/*` which consumes a lot of disk space.
- To clean the space use the command :
  
```bash
  sudo journactl --rotate
  sudo journalctl --vacuum-time=1week  ## delete all files older that one week
```

![[Pasted image 20240418163022.png]]

- For more informations look at [[Log File Administration]].

# 56.

- Answer is *D*.
- - To display the Informations about the user, such as *password expiration date* and more, enter the command :
  
```bash
  chage -l username
```
![[Pasted image 20240419122427.png]]

- More in [[Administering Users and Groups]].

# 57.

- Answer is *B*.
- For more in [[Remote Administration (SSH)]].

# 59.

- Answer is *A*.
  ![[Pasted image 20240419123328.png]]
  
- Here is the options of the `sudo` :
  ![[Pasted image 20240419123351.png]]

# 60.

- Answer is *B*.
- More in [[sudo]]

# 62.

- Answer is *A*.
- Options to `hosts.deny` :
	- Single IP address (Rejects the connection attempt with an ICMP response):
	
	- `REJECT: 192.168.1.100`
	    
	- IP address with subnet mask (Similar to `ALL`, blocks all connection attempts):
	    
	    
	- `DENY: 192.168.0.0/24`
	    
	- IP address with subnet mask in CIDR notation (Rejects the connection attempt with an ICMP response):
	    
	    
	- `REJECT: 10.0.0.0/255.255.255.0`
	    
	- Hostname (Treats the connection attempt as an error, potentially logging it):
	
	`ERROR: example.com`

- More in [[Security]].

# 64. 

- Answer is *C*.
- Look at [[sudo]].

# 65.

- Answer is *C*.
- Look at [[Security]].

# 67.

- Answer is *B*.
- The ssh keys are stored in the ==Server== in the `/etc/ssh_known_hosts` ! because the server handels the keys system wide and could have more than one key !
- More in [[Remote Administration (SSH)]].

# 70.

- Answer is *B*.
-  - The Key is stored as `pubring.gpg` in `~/.gnupg`  ![[Pasted image 20240419133159.png]]

- More in [[Security]].

# 72.

- Answer is *A*.
- More in [[ENABLING SSL-TLS IN APACHE]].

# 73.

- Answer is *B*.

![[Pasted image 20240419134209.png]]

![[Pasted image 20240419134227.png]]

- More in [[Linux/Network Configuration/File Sharing Services|File Sharing Services]].

# 76.

- Answer is *C*.
- For more look for [[PAM]].

# 77.

- Answer is *C*.
- More in [[PAM]].

# 78.

- Answer is *B*

# 79.

- Answer is *B*.
- PEM (Privacy Enhanced Mail) is a widely used format for storing cryptographic objects such as certificates and private keys.
# 83.

- Answer is *D*.
### fail2ban config file

- Go to the file `vim /etc/fail2ban/fail2ban.conf` and make the changes you want :
  ![[Pasted image 20240415132729.png]]

- More in [[fail2ban]].

# 84.

- Answer is *B*.
![[Pasted image 20240420114229.png]]

- More in [[Remote Administration (SSH)]].

# 100.

- The answer is *B*.

## Enabling SELinux

- SELinux have 3 Modes :
  
  - *Enforcing or 1* : denies access to users based on the SELinux policies.
  - *Permissive or 0* : record logs of actions and don't block the users.
  - *Disabled* : dis-activate SELinux.

- To change the mode, use the `getenforce >Mode<` command :
  ![[Pasted image 20240326180300.png]]
- This modes are present in the `/etc/selinux/config` file :
  ![[Pasted image 20240326180433.png]]


> [!Important]
> 
> - The only way to *Disable* SELinux, is by changing the `SELINUX=disabled` in the `/etc/selinux/default` !


- More in [[SELinux]].

# 101.

- Answer is *A*.
- The Linux System normally uses the *DAC (Directory Access Control)*, which says, that the owner of a file decides who can access the file.
- More in [[SELinux]].

# 103.

- Answer is *D*.

   - `<control flag>` : action that PAM should make based on the result of the module.
     
      - `required` : the module's success is required for the authentication.If the module fails, the user authentication fails *but other processes continue to run*. The user is notified if the module interface are finished.
      - `requisite` : the same as required, but if the module fails, PAM will immediately terminate and notify the user about the failure.
      - `sufficient` : if the module fails the authentication continues with other modules.
      - `optional` : Success and failure of the module, doesn't affect the authentication; module is ignored.

- More in [[PAM]].

# 104.

- Answer is *B*.
- The root user has the UID of *0*.
  ![[Pasted image 20240420122803.png]]
# 107.

- The Answer is *D*.
![[Pasted image 20240420123610.png]]

- Normaly the line starts with `password`.
- More in [[Password protected Grub]].

# 108.

- Answer is *B*.
- SELinux have 3 Modes :
  
  - *Enforcing or 1* : denies access to users based on the SELinux policies.
  - *Permissive or 0* : record logs of actions and don't block the users.
  - *Disabled* : dis-activate SELinux.

- More in [[SELinux]].

# 109.

- Answer is *C*.
  
> [!Important]
> - First enable the ssh-agent :
> ```
> exec ssh-agent bash  ---> run ssh-agent
> ```
> - To test the ssh-agent :
>   ![[Pasted image 20240307130559.png]]
> 
> - Next add the *privat* key to the ssh agent : `ssh-add $HOME/.ssh/id_ed25519`.
> - know you can run log to your remote machine with SSH :
>   ![[Pasted image 20240307130403.png]]
> - The `ssh-agent` is a program that runs in the background and stores your decrypted private keys, allowing you to authenticate to remote servers using SSH without having to re-enter your passphrase every time.

- More in [[Remote Administration (SSH)]].

# 111.

- Answer is *C*.
- to provide a special username and other parameters related to a specific host to which you connect using SSH :
![[Pasted image 20240420130446.png]]

# 113.

- Answer is *B*.
  
> [!Important]
> - The settings are lost when rebooting the system. to make them permanent, use the command :
>   
> ```
>   setsebool -P
> ```

- More in [[SELinux]]

# 114.

- Answer is *B*.
- The profiles and application permissions of the apparmor are located in `/etc/apparmor.d/`:
  ![[Pasted image 20240420130859.png]]

# 118.

- Answer is *C*.
- More in the *Video* from [IT Pro TV](https://www.youtube.com/watch?v=C0lpb6mwCRE)
- To list all the apps, that are using Network Sockets, and have open Ports, AppArmor list the as *Unconfined*. to List them use the command :

```
aa-unconfined
```
![[Pasted image 20240328145645.png]]

- We have now this list of Software that are needed to be confined.
- More in [[AppArmor]].

# 120.

- Answer is *C*.
- To change the context of a file we use the command `chcon`.
  - Lets say, we want to change the content *type* of the `adir` directory from `admin_home_t` to `unconfined_t` we use the command :
    
	```bash
    chcon -R -r unconfined_r -t unconfined_t ./adir
	```
	
	![[Pasted image 20240326183804.png]]

- More about `chcon` :
  ![[Pasted image 20240326184005.png]]

- To get more informations about the attributes of SELinux, use the command `seinfo`.
  - `seinfo -t` : will display informations about the types.
  - `seinfo -r` : will display the informations about the role ...
- More in [[SELinux]]

# 127.

- Answer is *A*.

## Switching Modes

- If you want to switch to *complain mode*, use the command `aa-complain` :
  
```bash
  [root@localhost ~]# aa-complain /etc/apparmor.d/*
```

- To switch to *enforce mode* use the command `aa-enforce` :
  
```bash
  [root@localhost ~]# aa-enforce /etc/apparmor.d/*
```

- To disable a profile, use the `aa-disable` :

```bash
[root@localhost ~]# aa-disable /etc/apparmor.d/usr.bin.cupsd
Disabling /etc/apparmor.d/usr.sbin.cupsd
```

- More in [[SELinux]].


# 128.

- Answer is *B*.
- Add a SSH-Rule to a sepcific user with `Match User` under the file `/etc/ssh/sshd_config`:
  
```bash
  PasswordAuthentication no
...

Match User admin
        PasswordAuthentication yes
```


# 136.

- Answer is *C*.
- To run a shell (scripted env) in a *non-interractive Mode*, use the `sudo -n`
  ![[Pasted image 20240421130128.png]]

# 148.

- Answer is *t*.

- To ==List== the table name use the option `-t`.

![[Pasted image 20240421131013.png]]

- More in [[Iptables]].

# 158.

- Answer is *B*.
- The module `account` is responsible for the accounts.
- `<module interface>` : Defines the type of PAM group. they are 4 :
     
     - `auth` : handles user authentication (Kerberos)
     - `account` : manages accounts; checking permissions, account validity.
     - `password` : changing and updating passwords.
     - `session` : manages opening and closing sessions and setting up environments (home directory).

# 168.

- Answer is *B*.
- More in the [Wiki](https://wiki.gentoo.org/wiki/Rsyslog).
- To enable the rsyslog to run the module `imudp` within the port `514`, you need to add this to `/etc/rsyslog.conf` :
  
```bash
$ModLoad imudp
$UDPServerRun 514
```

# 169.

- Answer is *A*.
- look for the `journald.conf` in the `/etc/systemd/journald.conf` :
  ![[Pasted image 20240423132340.png]]

- Here you can addjust the maximum size of the individual journal logs `SystemMaxFileSize`.

# 170.

- Answer is *D*.
- More in [[Log File Administration]].

# 171.

- Answer is *C*.
- If you modify The `/etc/issue` would look like this :
  ![[Pasted image 20240425140338.png]]


# 172.

- Answer is *C*.
- Editing the `/etc/motd` will add messages, when users logged in to a Server :
    ![[Pasted image 20240425141209.png]]

- More in [[Change Display Messages]].

# 173.

- Answer is  *C*.
- The `realm` command discovers the and join the Active Directory domain, while using `sssd` in Linux.
- More in [[Security]].

# 175.

- Answer is *C*.

> [!Important]
> - All this Rules for Users :
>   
>   
>   - Rules for the Password.
>   - Setting the Home Directory.
>   - Groups.
>   - Encrytion method.
>   - Login Restrictions ...
>     
>     are set in the `/etc/login.defs` directory :
>     ![[Pasted image 20240425143443.png]]
>   ![[Pasted image 20240425143533.png]]


- More in [[Administering Users and Groups]]

# 178.

- Answer is *D*.
- More in [[Remote Administration (SSH)]].

# 182.

- Answer is *C*.
- More in [[SELinux]].

# 185.

- Answer is *C*.
- `pkexec` enables you to run a command as another user :
  ![[Pasted image 20240425152637.png]]

# 188.

- Answer is *A*.
![[Pasted image 20240425153121.png]]

