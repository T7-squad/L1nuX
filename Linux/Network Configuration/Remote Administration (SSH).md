## Secure Shell (SSH)

### Understanding SSH Host and User Keys

- SSH *Host Keys* are stores the *private* and *public* key in the `/etc/ssh` :
  ![[Pasted image 20240305142358.png]]
- Here are 4 types of keys Encrypted (DSA, RSA, ECDSA, and ED25519).
- The `.pub` are the *public* key, that each machine get when attempting to connect to another machine.
- The file without `.pub` is the *private* key. Which is very important to encrypt and decrypt the public key. 
- When you want to connect to another machine, You will prompt to accept the *public* key of the other machine, this key can be found in the `~/.ssh/known_hosts`.
- If you reconnect to the same machine one more time, most of the time you need to remove the old public keys of the old machine.

> [!Important]
> - To generate a new *private and public* ENCRYPTION key for a particular encryption type, you can use the command `ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -t ecdsa` . this will store the ssh private key in the giving path and encrypt the key with the ECDSA Encryption.
> - This is *NOT* the exchanged Key !!

#### Generating a SSH User key

- You can generate a *public* and *private* key for a user account with the command `ssh-keygen`.
  ![[Pasted image 20240419122703.png]]
  
- The *SSH user keys* are stored in the `~/.ssh` directory.
- The `~/.ssh` directory contains the files `~/.ssh/id_rsa.pub` which is the public key (can be copied to the target machine and used instead of a password ) and the `~/.ssh/id_rsa` key which is the private key (to encrypt the public key ).
- To copy the public key to another machine you use the command `ssh-copy-id` :
  ![[Pasted image 20240305145717.png]]

- The target machine will store this key in the `~/.ssh/authorized_keys` for each user account.
- Example :
  
```BASH
[root@server1 ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:lZu0U5EsQWcM1dFWNHvYK07YB5D8uQhgp/XuRACP5PQ root@server1
The keys randomart image is:
+---[RSA 2048]----+
|
+..=B*o.=+|
|
+o++.*=..o=|
|
.o=E*.o.ooo|
|
. + Boo. o|
|
S O..+.o |
|
=o.o |
|
o .
|
|
.
|
|
|
+----[SHA256]-----+

[root@server1 ~] # ls .ssh

id_rsa id_rsa.pub known_hosts

[root@server1 ~]# ssh-copy-id root@server2

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed:
"/root/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out
any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now
it is to install the new keys
root@server2s password: **********
Number of key(s) added: 1
Now try logging into the machine, with:
"ssh 'root@server2'"
and check to make sure that only the key(s) you wanted were added.

[root@server1 ~]# ssh root@server2

Last login: Sun Oct 14 20:17:41 2023 from server1

[root@server2 ~]# ls .ssh
authorized_keys
[root@server2 ~]#_
```

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


> [!Important]
> - The ssh keys are stored in the ==Server== in the `/etc/ssh_known_hosts` ! because the server handels the keys system wide and could have more than one key !

![[Pasted image 20240421125013.png]]

### Configuring SSH

- To configure the SSH daemon to use the exchanged keys you muss edit the `/etc/ssh/sshd_config` file and add `no` to `PasswordAuthentication` : 
  ![[Pasted image 20240305150520.png]]

- To connect directly to the root user, you need to change the `PermitRootLogin` to `yes`.

> [!Important]
> - To do PortForwarding for SSH you uncomment this line :
>   
> ```bash
>   AllowTcpForwarding yes
> ```

- Add a SSH-Rule to a sepcific user with `Match User` under the file `/etc/ssh/sshd_config`:
  
```bash
  PasswordAuthentication no
...

Match User admin
        PasswordAuthentication yes
```

## Using Socks Proxy with SSH

- Socks Proxy allows you to tunnel your SSH connection to the Host Server.
- SSH-Socks proxy can be used to **unmask your IP Address**.
- Anonymous Browsing.
- SSH with socks proxy enables you to forward your Trafic through the SSH Tunnel providing an encryption and potential **bypassing network restrictions**.
- In this case, your machine acts like a *proxy server*.
- To open the SSH Proxy Server :
  
```bash
  ssh -D <local_port> <username>@<remote_host>
```
This will open an SSH connection, and starts the socks proxy Server on your local machine.

- To configure applications to use the Socks Proxy, you can go the your browser for example, and set the browser to use the Socks Server.
- You can Use your Email Client, FTP Client, Wget/Curl ... to use the Socks proxy.
- 

# Port Forwarding with SSH

## The difference between normal SSH and Port FOrwarding with SSH

A normal SSH connection is used to establish a secure, encrypted connection between two machines, typically for remote access or file transfer. When you establish a normal SSH connection, all data transmitted between the two machines is encrypted and secure from eavesdropping or tampering.

SSH port forwarding, on the other hand, is used to forward network traffic from one machine to another through an encrypted SSH tunnel. This allows you to securely access network services or resources that would otherwise be inaccessible due to firewalls or other security measures. With SSH port forwarding, you can redirect traffic from one port on your local machine to another port on a remote machine through the encrypted SSH tunnel.

## Local Port Forwarding

==With local port forwarding, you are forwarding traffic from a port on your local machine to a port on a remote server==. This means that any data transmitted to the forwarded port on your local machine is encrypted and sent over the SSH connection to the remote server, which then forwards the encrypted data to the specified destination on its network.

For example, let's say you want to access a web server running on a remote server, but the server is not directly accessible from your local machine because of a firewall or network configuration. You can use SSH port forwarding to create a secure tunnel between your local machine and the remote server, allowing you to access the web server as if it were running on your local machine.

To do this, you would use the following command on your local machine:

```Python
ssh -L 8080:localhost:80 username@remote-server
```

Here, we are using the `-L` option to specify local port forwarding. The command will create a secure SSH connection to the remote server and forward traffic from port 8080 on your local machine to port 80 on the remote server. The `username` is the username you use to log in to the remote server.

Once the SSH connection is established, you can access the web server by opening a web browser and navigating to `http://localhost:8080`.

## Remote Port forwarding

==with remote port forwarding, you are forwarding traffic from a port on a remote server to a port on your local machine==. This means that any data transmitted to the forwarded port on the remote server is encrypted and sent over the SSH connection to your local machine, which then forwards the decrypted data to the specified destination on your local network.

For example, if you use remote port forwarding to forward traffic from port 3306 on a remote database server to port 3306 on your local machine, any requests made to `localhost:3306` on the remote server will be encrypted and forwarded to your local machine, which will then decrypt the data and forward it to the local MySQL server.

you can use remote port forwarding to allow traffic from a remote server to be forwarded to a port on your local machine. For example, let's say you have a database server running on your local machine and you want to access it from a remote server. You can use the following command on the remote server:

```Python
ssh -R 3306:localhost:3306 username@local-machine
```

Here, we are using the `-R` option to specify remote port forwarding. The command will create a secure SSH connection to the local machine and forward traffic from port 3306 on the remote server to port 3306 on your local machine. The `username` is the username you use to log in to the local machine.

![image](forw.png)

- In the Image shown above, we used SSH portforwarding on the POrt 22 because its not blocked by the FIrewall by default. This means we are able to acces our Server with an SSH portforwarding on Port 22
- In our C2 set up from Task 4, our Teamserver is listening on localhost on TCP/55553. In order to access Remote port 55553, we must set up a Local port-forward to forward our local port to the remote Teamserver server. We can do this with the -L flag on our SSH client:

SSH Port Forward :

```Python
root@kali$ ssh -L 55553:127.0.0.1:55553 root@192.168.0.44 
root@kali$ echo "Connected"  
Connected
```

## How hackers use Portforwarding 

SSH port forwarding is a feature of the SSH protocol that allows users to forward traffic from one network port to another through an encrypted tunnel.

To give you some examples of traffic that can be forwarded using SSH port forwarding:

1.  Remote desktop connections: Users can forward traffic from a remote desktop port (e.g., port 3389 for Windows Remote Desktop Protocol) to a local machine, allowing them to access the remote desktop as if they were sitting in front of it.
2.  Web servers: Users can forward traffic from a remote web server (e.g., port 80 for HTTP traffic) to a local machine, allowing them to access the web server's content as if it were hosted locally.
3.  Database servers: Users can forward traffic from a remote database server (e.g., port 3306 for MySQL) to a local machine, allowing them to access the database as if it were hosted locally.
4.  Secure shell (SSH) connections: Users can forward traffic from a remote SSH server (e.g., port 22) to a local machine, allowing them to establish an SSH connection to the remote server as if they were on the same network.

Exemples :

To access the local machine, you can use SSH port forwarding as follows:

1.  Connect to a remote server that has SSH access to the local network, using the `ssh` command in your terminal:

```Python
ssh remote_server_username@remote_server_ip_address
```
2.  Once you're connected to the remote server, set up port forwarding to forward traffic from the RDP port (3389) on the local machine to a port on your remote machine (let's say port 5555), using the `-L` option:

```Pyhton
ssh -L 5555:local_machine_ip_address:3389 remote_server_username@remote_server_ip_address
```

Here, `local_machine_ip_address` is the IP address of the machine you want to access on your local network.

3.  Once the port forwarding is set up, open the Remote Desktop Connection client on your remote machine and connect to `localhost:5555`. The RDP traffic will be forwarded through the SSH tunnel to your remote machine, which will then connect to the local machine on your behalf.

As for some examples in bash, here are a few more examples of SSH port forwarding using the `ssh` command:

1.  Forwarding HTTP traffic from a remote web server to a local machine:

```Python
ssh -L 8080:remote_web_server_ip_address:80 remote_server_username@remote_server_ip_address
```

This will forward traffic from the remote web server's port 80 to port 8080 on your local machine.

2.  Forwarding MySQL traffic from a remote database server to a local machine:

```Python
ssh -L 3306:remote_database_server_ip_address:3306 remote_server_username@remote_server_ip_address
```