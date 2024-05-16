
- What you need to know to install a Linux Server :
  - *Hostname and IP configuration of the Server :* To enter the command line of the Server you need the Hostname and IP of the Server. The IP Address of the Server is more often Static !
  - *Automatic Updating ? :* As you know, updating your Linux Server may cause package corruption which can damage your Server. Rather then make the update each day, you can set a particular time and date for the update to be lunched !

## Dealing with Problems during the Installation 


> [!Overclocked] 
> An *overclocked* CPU is a CPU that is faster than the speed for which the processor was designed.
Although this might lead to increased performance, it also makes the processor hotter and can result
in intermittent computer crashes.


## Dealing with Problems after the Installation

- Some of the Problems that can cause the Server to not boot can be different. It can be a Hardware, Driver Issues ...
- To troubleshoot this problems, you need to look at :

### Viewing Installation Logs

- To see if the installation tasks were performed successfully you can check the *installation log files*, immediately after the installation.
- Log files stores all the events and errors recorded while installing something.
- To see the Log Files : `cat /var/log/apt/history.log` :
  ![[Pasted image 20240215192824.png]]
- To see errors from the log file : `grep –i "(error|warn) /var/log/apt/history.log`
- More Log files :
![[Pasted image 20240215195355.png]]
![[Pasted image 20240215195417.png]]

