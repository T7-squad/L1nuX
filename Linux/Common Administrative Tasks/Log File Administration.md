
- To identify and troubleshoot an linux system, you muss keep track of the errors that occur during an event.
- Most of daemons record informations and errors in *log* files.
- The most of this log files are situated in the `/var/log` Directory.
- Here are some common linux log files found in `/var/log` Directory :
  
 ![[Pasted image 20240225205924.png]]

- The two most common logging daemons used on Linux systems today are the *System Log Daemon (rsyslogd)* and the *Systemd Journal Daemon (journald)*.

## Working with the System Log Daemon (rsyslogd)

- The System Log Daemon is the default login daemon used in Linux.
- When is loaded at startup it created a socket (`/dev/log`) for other system processes. It reads any event and record it according to `/etc/rsyslog.conf` and any .conf file in the `/etc/rsyslog.d`.
- More in the [Wiki](https://wiki.gentoo.org/wiki/Rsyslog).
- To enable the rsyslog to run the module `imudp` within the port `514`, you need to add this to `/etc/rsyslog.conf` :
  
```bash
$ModLoad imudp
$UDPServerRun 514
```


## Automatic rotation (archive) of logfiles ( also emailing)

- The tool used for is the `logrotate`.
- More about the tool :
  ![[Pasted image 20240423133338.png]]

- go to `/etc/logrotate.conf` to see the configuration file :
  ![[Pasted image 20240423133453.png]]

- Example of scripts to enter in the `/etc/logrotate.conf` :
  ![[Pasted image 20240423133709.png]]

- To enable compression use `compress`, to disable it use `nocompress`.
  

## Working with the Systemd Journal Daemon

- More about the journalctl in [[Installing a Linux Server Distribution]] .
- You can view the log file of a particular process (Program) by typing : `sudo journalctl _COMM=` and pressing the TAB key (! this only works on Bash !)  
  ![[Pasted image 20240226121146.png]]

- After that you choose your Process to troubleshoot, example : `dbus-daemon` :
  `sudo journalctl _COMM=dbus-daemon`
  ![[Pasted image 20240226121431.png]]

- The time format of the `journalctl` follows the standard *YYYY-MM-DD HH:MM:SS* like *2023-08-22 17:30:00* : `sudo journalctl _COMM=crond --since="2023-08-22 17:30:00"` .
- The `journalctl` can filter the output in Units, example `sudo journalctl --unit=crond.service`.
  - You can search for units with `sudo systemctl list-units | grep >your service<` .
  - Once you found you service that you want to troubleshoot, you can then use the command `sudo journalctl --unit=virtualbox-guest-utils.service` :
    ![[Pasted image 20240226155313.png]]


> [!Important]
> 
> - To display a live messages from the journalctl use the option `-f`:
>   
> ```
> journalctl -f 
> ```

### Configuring a .service file 

- [Here](https://manpages.debian.org/testing/manpages-de/systemd.unit.5.de.html) List of the Systemd units ! or type `man systemd.unit`.

- Under `[Unit]` :

1. `Description=`: A human-readable description of the unit.
2. `Requires=`: Defines units that this unit depends on.
3. `After=`: Specifies units that this unit should start after.
4. `Before=`: Specifies units that this unit should start before.
5. `Wants=`: Similar to `Requires`, but the unit won't fail if the dependencies are not met.
6. `WantedBy=`: Specifies target units that should activate this unit.
7. `RequiredBy=`: Specifies target units that require this unit.
8. `PartOf=`: Specifies units that this unit is part of.
9. `Conflicts=`: Specifies units that conflict with this unit.
10. `Before=`: Specifies units that this unit should start before.
11. `After=`: Specifies units that this unit should start after.

Under `[Service]`:

1. `Type=`: Specifies the type of the service (simple, forking, oneshot, *nfs* etc.).
2. `ExecStart=`: Specifies the command to start the service.
3. `ExecStop=`: Specifies the command to stop the service.
4. `ExecReload=`: Specifies the command to reload the configuration.
5. `Restart=`: Specifies the conditions under which the service should be restarted.
6. `RestartSec=`: Specifies the time to wait before restarting the service.
7. `TimeoutStartSec=`: Specifies the time to wait for the service to start.
8. `TimeoutStopSec=`: Specifies the time to wait for the service to stop.
9. `Environment=`: Sets environment variables for the service.
10. `WorkingDirectory=`: Specifies the working directory for the service.
11. `User=` and `Group=`: Specifies the user and group under which the service should run.
12. `PermissionsStartOnly=`, `PermissionsStartPost=`, `PermissionsStopOnly=`, `PermissionsStopPost=`: Specifies permissions for service startup and shutdown.
13. `StandardOutput=` and `StandardError=`: Specifies where to direct the standard output and error streams.
### Deleting the older journalctl archive files

- As you use your machine, the journalctl keeps track of your machine and stores the data in the `/var/log/journal/*` which consumes a lot of disk space.
- To clean the space use the command :
  
```bash
  sudo journactl --rotate
  sudo journalctl --vacuum-time=1week  ## delete all files older that one week
```

![[Pasted image 20240418163022.png]]

- To display the Disk-Space related to the journalctl, run the command :
  
```bash
  journalctl --disk-usage
```

![[Pasted image 20240505130912.png]]



