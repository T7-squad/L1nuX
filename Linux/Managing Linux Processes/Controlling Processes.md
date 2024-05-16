

### Sending Signals to kill Processes

- The common syntax for killing Processes : `$kill -signal PID `
- The following signals are commonly used to kill a process in Linux:
	
	1.  `SIGTERM` (signal `15`) - Request process termination.
	2.  `SIGKILL` (signal `9`) - Forced termination, cannot be ignored.
	3.  `SIGINT` (signal `2`) - Interrupt from keyboard.
	4.  `SIGHUP` (signal `1`) - Hang up or disconnect.
	5.  `SIGQUIT` (signal `3`) - Quit and dump core.

### Shutting down the System

These are some common commands used for shutting down or rebooting a Linux system:

1.  halt: Shuts down the system and stops all processes:`halt`
2.  poweroff: Same as halt, but sends an ACPI signal to power off the system:`poweroff`
3.  reboot: Restarts the system:`reboot`
4.  shutdown: Schedules a time for the system to halt or reboot
		`shutdown [OPTION] [TIME] [MESSAGE]`
	- Options: -r: reboot the system -h: halt the system
	- `$shutdown -r now "System is restarting" 
	- `$shutdown -h +5 "System will halt in 5 minutes"`

## Show all services running 

- To show all the services running on you machine type : 

```shell
systemctl list-units --type=service --state=running
```

