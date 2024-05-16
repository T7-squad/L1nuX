# Samba

- Look for [[Enumeration]], [[Windows File Transfer Methods]] 
- The Samba sharing service uses the *SMB* protocol.
- To share files with windows, you can use the `smbd` daemon.
- A lot of computers uses the *NetBios* protocol.
  - For the case of NetBios, you can use the daemon `nmbd`.

## Configuring a Samba Server

- More in the Arch Wiki [Samba](https://wiki.archlinux.org/title/samba), Or the Debian Wiki [Samba](https://wiki.debian.org/Samba/ServerSimple).
- First you have to create a list of Windows/Linux Users in your Linux machine and add them to the SMB-SHARE Group.
- To add a user to the SMB-Share use the command `smbpasswd` :
  ![[Pasted image 20240308120207.png]]
  ![[Pasted image 20240313143946.png]]

- Take a look in the log file in `/etc/samba/smb.conf` :
  ![[Pasted image 20240308120525.png]]

- To test the config file for syntax errors, run the following command :`testparm` .
- You can add more secure configurations to the Samba Server :
  
  1. Allowed/Not Allowed IPs :
     
```sql
[global]

hosts allow = 192.168.1.0/24 10.0.0.1 ...
hosts deny = 0.0.0.0

```


### Setting up a SAMBA Server localy

1. First install `smbclient` and `samba` on both machines.
2. Add a user in the Server Machine to the Samba Group :
  ![[Pasted image 20240313143946.png]]

3. Create a directory to be shared :
   
```
mkdir /home/ubuntu/Desktop/SHARED 
```

4. Make a backup of the `/etc/samba/smb.conf` to `~` in case to fucked it up !
5. Edit the `/etc/samba/smb.conf` at the bottom :
   
```
[<folder_name>]
path = /home/ubuntu/SHARED
valid users = <user_name>
read only = no
```

6. restart the service with : `sudo systemctl restart smb`.
7. Check for Errors with : `testparm`.
8. Access the Share with :
   
```
smbclient -L //<HOST_IP_OR_NAME>/<folder_name> -U <user>
```

9. (On the VBox Machine) Forward the Port *445* to *4445* in the local machine and connect to the SMB-Server :
   ![[Pasted image 20240313153518.png]]

10. To connect to the SMB-SHARE, use the command :
    
```
smbclient //<HOST_IP_OR_NAME>/<folder_name> -U <user> -p <Port>
```

![[Pasted image 20240313154011.png]]

11. You can also *Put* some files from the Local machine to the SMB-SHARE with `put` :
    ![[Pasted image 20240313154538.png]]


## Connecting to a Samba Server

- To Access the SMB Server from **Windows**, browse to `\\server`, where `server` is the NetBios name or the IP Address of your Samba Server.
- To connect to SMB Server from **Linux**, browse to : `smb://server` or `cifs://server`. Or you can use the command `smbclient -L` command to list the contents of the server :
  ![[Pasted image 20240308121806.png]]

- To connect to the SMB-SHARE, use the command :
    
```
smbclient //<HOST_IP_OR_NAME>/<folder_name> -U <user> -p <Port>
```

![[Pasted image 20240313154011.png]]

 - You can also *Put* some files from the Local machine to the SMB-SHARE with `put` :
    ![[Pasted image 20240313154538.png]]


- You can connect to the SMB-SHARE via the GUI :
  ![[Pasted image 20240313155005.png]]
  
  - Enter the Username and Password.
  - And here you are :
    ![[Pasted image 20240313155056.png]]


### The `net` Command 

![[Pasted image 20240419134209.png]]

![[Pasted image 20240419134227.png]]

# NFS

- More in the Arch Wiki [NFS](https://wiki.archlinux.org/title/NFS).
- *NFS* stands for the *Network File System*.
- Its a cross platform software.
- You need first to install the NFS Server with the command :
  
	`sudo apt install nfs-kernel-server`
	
- NFS shares the files that are placed in the `/etc/exports` File :
  ![[Pasted image 20240308124626.png]]

- Other computers can access the shared files and directories by mounting them.

## Configuring a Linux NFS Server

### This is How you do it 

- For more look at [NFS](https://ubuntu.com/server/docs/service-nfs).
- To configure the NFS Server, you need to edit the `/etc/exports` file.
- To share a directory named `source`, to allow a user `server1` to connect to with the permission of read and write `rw`, while treating the user as an anonymous user on the NFS Server, as well as the share `/home/ubuntu/Desktop/NFS` directory to all users allowing them to read and write the data `rw` :
  
```
/home/ubuntu/Desktop/NFS    *(rw,rsync)
```

> [!NOTE 1]
> - Another Example :
>   
> ```bash
> /srv/nfs        192.168.1.0/24(rw,fsid=root)
> /srv/nfs/music  192.168.1.0/24(rw,sync)
> /srv/nfs/home   192.168.1.0/24(rw,sync)
> ```
> 

> [!NOTE 2]
> 
> - In General :
>   ![[Pasted image 20240313160754.png]]

  
- After making this changes you have to update the `/etc/exports` with the command :

```bash
  exportfs -arv
```

- Then restart the service with `systemctl restart nfs-kernel-server.service`
- The NFS Server operate on the port *2049* :
  ![[Pasted image 20240313160643.png]]
- Install the utility : `sudo apt install nfs-common`.
- If you are on a virtual Box, forward the Port *2049* for NFS.
- Next, make a new Directory, example `mkdir /mnt/nfs`.
- mount the Shit : 
  
```bash
sudo mount -t nfs -o port=22049 127.0.0.1:/home/ubuntu/Desktop/NFS /mnt/nfs

```

- cd to `/mnt/nfs`.
- Now you have it ! 
  ![[Pasted image 20240313165825.png]]

  
### Mount the directories 

1. The NFS root is `/srv/nfs`.
2. The export is `/srv/nfs/music` via a bind mount to the actual target `/mnt/music`.

```
mkdir -p /srv/nfs/music /mnt/music
mount --bind /mnt/music /srv/nfs/music

```

- To make the bind mount persistent across reboots, add it to [fstab](https://wiki.archlinux.org/title/Fstab "Fstab"):

```
/etc/fstab

/mnt/music /srv/nfs/music  none   bind   0   0
```

- Or :

```
servername:/music   /mountpoint/on/client   nfs defaults,timeo=900,retrans=5,_netdev	0 0
```

### Restricting NFS to interfaces/IPs

- By default, starting `nfs-server.service` will listen for connectiocatns on all network interfaces, regardless of `/etc/exports`. This can be changed by defining which IPs and/or hostnames to listen on.
- To add the IP Address allowed to listen on the Share you need to edit the `/etc/nfs.conf` :
  
```bash
[nfsd]
host=192.168.1.123
# Alternatively, use the hostname.
# host=myhostname
```

- Then restart the service by `sudo systmctl restart nfs-server.service`.

## Connecting to a Linux NFS Server

- To list all the shared directories on a server (lets say `nfs.sampledomain.com` ) you use the command `showmount` :
  ![[Pasted image 20240308130658.png]]
  ![[Pasted image 20240308130721.png]]

- The `/var` directory is shared with all users (`*`).
- To mount the shared Directory to your local machine, you use the `mount` command with `-t nfs` to specify the `nfs` type :
  
```bash
  mount -t nfs nfs.sampledomain.com:/var /mnt
```

- Or in General :

```
# mount -t nfs servername:/music /mountpoint/on/client
```

