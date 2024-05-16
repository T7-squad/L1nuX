

> [!NOTE] Changing tty on the command line :
> - To change to tty on the command line, type the command `sudo chvt >number<`

- The boot loader loads the Kernel into memory, the kernel then executes the *init* daemon which execute other daemons and brings the system into usable state.
- Traditional Linux system have used the *UNIX SysV* process. but now all modern Linux systems adopted the *Systemd* system initialization process.
- The init daemon is responsible for stoping and starting daemons.

## RunTime /etc/inittab Fileevels

- *Runlevel* represent the number and the type of daemons that are loaded into memory and executed by the kernel.
- These are the Linux Runslevels :

![[Pasted image 20240218220934.png]]
![[Pasted image 20240218221416.png]]

- To set a *runlevel*, enter the command `init <Runlevel>` (Runlevel can variate from 0 to 6).
- You can switch the runlevels while you are using the Desktop Environment.
- `sudo init 1` switch to the single user mode, more in [[Single User Mode]].
  - Or you just can add `single` to the kernel line within the GRUB configuration screen :
    ![[Pasted image 20240219125403.png]]
    And press Ctrl+x to boot to single Mode.

- Another Way to enter the debugging shell is to add the line :
  
```
  systemd.debug-shell=1
```

and run the kernel.


## The /etc/inittab File

- The `/etc/inittab` file contains the default configurations of the init file :
```bash
[root@server1 ~] # cat /etc/inittab
id:5:initdefault:
[root@server1 ~]#_
```

- Runlevel 5 is the default Runlevel of all linux systems.

## Runtime Configuration Scripts

- During the boot time, the init process execute several scripts that prepare the system to be used.
- This scripts are called *runtime configuratin scripts (rc)*.
- This scripts are stored in the `/etc/rc#.d` (# is the runlevel number).
- For the most of the Linux Systems, the runtime scripts are located in `/etc/rc5.d` :
![[Pasted image 20240219130409.png]]
- If The symbolic links starts with an *S* that means, that the script *Starts* the process, and when the link starts with a *K* , that means that the script *Kills* the process.
- In the runlevel 5 (above), you can see the all the script are started at boot.
- In the runlevel 1 (Under), the most of the scripts that are started in the runlevel 5 are killed :
![[Pasted image 20240219131146.png]]
- In general :
![[Pasted image 20240219131234.png]]

- The next step is the that the init process, runs the scripts in the `/etc/init/*` Directory, to control the runlevels and prepare the Desktop Environment :
  
![[Pasted image 20240219132133.png]]

## Starting and Stopping Daemons Manually

- You can manipulate the daemons that are starting at system initialization by starting,stoping or restating them from the */etc/init.d* directory :
  
```bash
[root@server1 ~]$ /etc/init.d/cron restart
cron stop/waiting
cron start/running, process 3371
[root@server1 ~]
```

- You can also utilize the command ***service** command*, to start any daemon you want :
  
```bash
[root@server1 ~]$ service cron restart
cron stop/waiting
cron start/running, process 3352
[root@server1 ~]#
```

- You can list all running processes with `service --status-all` :
  ![[Pasted image 20240219164618.png]]

## Configuring Daemons to Start in a Runlevel

- You can manually configure your init system to start or stop a daemon at start.
- you do that with ***update-rc.d** command* .
- Here are some Examples :
  
  - To start a service at runtime 2 :
    
```
    sudo update-rc.d <service_name> start 2 .
```

  - To stop a service at runtime 2 :
    
```
    sudo update-rc.d <service_name> stop 2 .
```

 - Start apache2 in runlevels 2, 3, 4, and 5, and stop it in runlevels 0, 1, and 6 :
   
```
   sudo update-rc.d apache2 start 2 3 4 5 . stop 0 1 6 .
```

- To remove a process for example apache2, you need to run the command : `sudo update-rc.d -f apache2 remove`.
- `update-rc.d postfix defaults` will start the postfix daemon at runlevel 2 and 5 (default).
- Lets remove the service Bluetooth : 
  ![[Pasted image 20240219192443.png]]
  ![[Pasted image 20240219192527.png]]

## Working with Systemd

### Service, Target, Timers

- ==Service== represent the processes or the applications in Linux.
- To display the services run the command :
  
```bash
  sudo systemctl list-units --type=service
```

![[Pasted image 20240512113341.png]]

- ==Timer== is used to schedule and control the execution of services. It defines when the a service should be started :
  
```bash
  sudo systemctl list-units --type=timer

# OR :

  sudo systemctl list-timers
  
```
  ![[Pasted image 20240512113528.png]]


- ==Target== groups other units together. It represent a particular system, runlevel, multi-user-mode...
  
```bash
  sudo systemctl list-units --type=target
  
```

![[Pasted image 20240512113816.png]]



- Like UNIX SysV init daemon, the *systemd* is also responsible for starting/stoping daemons during the system initialization.
- Systemd Daemons are called *Service Units*, and runlevels are called *Target Units*
- Systemd services are located in the directory `/lib/systemd/system`, you can access them with `ls -la /lib/systemd/system`.
  ![[Pasted image 20240220131503.png]]
  
- Systemd Runlevels are mapped like so :
  ![[Pasted image 20240220131432.png]]![[Pasted image 20240220131503.png]]

- You can edit the sytemd services, for exemple, if you want to change the runlevel 3 to be the desktop target :
  
```bash
[root@server1 ~]$ ln -s /lib/systemd/system/runlevel3.target /etc/systemd/system/graphical.target
[root@server1 ~]#_
```

- If i multi-user-target want for example, the cron daemon, you can create a link from the cron service in the multi-user-target to the the actual cron.service :
  
```
  $ ln /lib/systemd/system/crond.service called /etc/systemd/system/multi-user.target
.wants/crond.service
```
![[Pasted image 20240220132712.png]]

 - To start/stop a daemon (cron for example) you can use the command : `sudo systemctl start/stop cond.service`.
 - If you made changes to the daemon, you use the `reload` option to reload the service.
 - To list all the services on the systemctl, you use either `sudo systemctl -a` or `sudo systemctl list-units` :
   ![[Pasted image 20240220133637.png]]

- You cann also filter the services :
  - If you are searching only for services that are running use the command : `sudo systemctl -a | grep running | less`.
  - If you are searching for services that are not-found : ``sudo systemctl -a | grep not-found | less`` 
    ![[Pasted image 20240220133917.png]]

- The command `systemctl enable crond.service` will create a link `/lib/systemd/system/crond.service` withing the `/etc/systemd/system/multi-user.target.wants` directory. This executes the crond service when the multi-user.target is started.


> [!NOTE] Note
> - To fully be sure that a service (cups for example) cannot be started you need to *mask* it with : `systemctl mask crond.service`. This redictes the link to `/dev/null`

- If you are in another runlevel like runlevel 3 and you want to switch to runlevel 5, type the command  :
  
    `systemctl isolate runlevel3.target`

### Enter emergency Mode 

- To make maintenance in your System, you can enter the *emergency mode* by entering the command :
  
```bash
  sudo systemctl emergency
```

![[Pasted image 20240512115820.png]]

- After you made your changes, you can switch back to the `graphical.target`, by typing the command :
  
```bash
  sudo systemctl isolate graphical.target
```

This will bring you back to the login screen!

### Show parameters of a service

- To inverstigate a service run the command :
  
```bash
  sudo systemctl show <service>
```

- Lets take the example `ssh.service` :
  ![[Pasted image 20240512111306.png]]

## Working with Systemd Unit Files

- Lets examin a systemd target unit file.

```bash
[root@server1 ~]$ cat /lib/systemd/system/graphical.target


[Unit]
Requires=multi-user.target
Wants=display-manager.service
Conflicts=rescue.target
After=multi-user.target display-manager.service
AllowIsolate=yes
[root@server1 ~]#_
```

- The `graphical.target` unit file has a single Unit section that:
	• `Requires` all services from the multi-user target be started; otherwise, the system will fail to switch to graphical.target.
	• `Wants` the display-manager service to be started; if display-manager cannot be started, switching to the graphical target will still succeed.
	• Cannot be run at the same time (`Conflicts`) as the rescue target.
	• Instructs Systemd to start the multi-user target and display-manager service before entering (`After`) the graphical target (`Before` would start these services after entering the graphical target).
	• Allow users to switch to the target (`AllowIsolate`) using the systemctl command following system initialization.

- Lets take now the service `crond.service` 

```bash
[root@server1 ~]$cat /lib/systemd/system/crond.service




[Unit]
Wants=network-online.target
After=network-online.target auditd.service systemd-user-
sessions.service
[Service]
Type=simple
User=root
EnvironmentFile=/etc/sysconfig/crond
ExecStart=/usr/sbin/crond
ExecStop=/bin/kill -INT $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=30s
[Install]
WantedBy=multi-user.target
[root@server1 ~]#_
```

- The crond.service unit file has a *Unit*, *Service*, and *Install* section that instructs Systemd to:
	• Ensure that the network is fully started before starting the crond service (`Wants=network-online.target`).
	• Start the network-online, auditd, and systemd-user-sessions services before starting (`After`) the crond service.
	• Treat the crond service as a regular daemon process (`simple`) started as the root user.
	• Load crond-specific environment settings from a file (`EnvironmentFile`).
	• Use specific commands to start, stop, and restart the crond service (`ExecStart`,` ExecStop`, and `ExecReload`).
	• Restart the crond service 30 seconds after a failure (`RestartSec=30s`).
	• Start the crond service when entering the multi-user target (`WantedBy`).


### Create a Timer Unit

- You can configure a *timer unit* to run a program periodically :
  
1. Create a `weekly-backup.service` file :
   
```
[Unit] 

Description=Weekly backup service after graphical desktop starts After=graphical.target 

[Service] 

Type=oneshot ExecStart=/bin/tar -czf /home/user/Documents_backup_$(date +\%Y\%m\%d).tar.gz /home/user/Documents/ 

[Install] 

WantedBy=graphical.target
```

2. Save the service File in the `/etc/systemd/system/` directory :`sudo mv weekly-backup.service /etc/systemd/system/`
3. Reload the daemon with `sudo systemctl daemon-reload`.
4. Enable the daemon with `sudo systemctl enable --now weekly-backup.service`.
5. and start it with `sudo systemctl start weekly-backup.service`.




 

