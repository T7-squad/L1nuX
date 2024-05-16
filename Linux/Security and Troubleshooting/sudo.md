
# Make a user run a command without sudo

- To make a user run a particular command without been prompt for the root password, you need to edit the `/etc/sudoers` like this :
  
```bash
john ALL=(ALL) NOPASSWD: /usr/sbin/service apache2 restart
```

# Run a command as another user

- To do that you need to run the command :
  
```
  sudo -u username
```

![[Pasted image 20240419123328.png]]
  
- Here is the options of the `sudo` :
  ![[Pasted image 20240419123351.png]]

- - To run a shell (scripted env) in a *non-interractive Mode*, use the `sudo -n`
  ![[Pasted image 20240421130128.png]]

# Run a command with `su`

- To run a command in a non-interractive shell, run the command :
  
```bash
  sudo su -c command
```
![[Pasted image 20240419131323.png]]
- To run a command in a particular shell, lets say the `bash` Shell run the command with `-s` or with `---shell` :

```
sudo su --shell=/bin/bash  
```

![[Pasted image 20240419131309.png]]