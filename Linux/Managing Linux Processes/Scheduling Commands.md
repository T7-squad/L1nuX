- There are two types of daemons to schedule commands in the future, *atd (at daemon)* and the *cron daemon (crond)* .

## Scheduling Commands with atd

- Here are some of the atd commands :
  ![[Pasted image 20240223130050.png]]

- First install the *at* package with `sudo apt-get install at -y`.
- Enter the command you want to run and press `Ctrl-D` to save and exit.
- Here is an Example :
  ![[Pasted image 20240223132340.png]]

- To remove a job, you run `atrm >job<`.
- To list the jobs, enter the command `at -l`.
- `at` will not work if the laptop is shut down !

## Scheduling Commands with cron

- User cron table format :
  ![[Pasted image 20240223135936.png]]
- Example : execute the script `/root/myscript` at 5:20 PM, and 5:40 PM Monday to Friday regardless of the day of the month of month of the year :
  ![[Pasted image 20240223140143.png]]

- To edit a crontab use the command `crontab -e`.

1. ~~Example 1 :~~

- Lets create a crontab that runs every day every month at 14:13 PM and executes the command `echo "Crontab !!" > /home/ubuntu/Desktop/hallo.txt` :
  - Lets create the cronjob with `crontab -e`.
  - Edit the cronjob :
    ![[Pasted image 20240223141623.png]]
  
  - The cronjob has been executed, you can see it by running the command : `journalctl -xe | grep Cron` :
    ![[Pasted image 20240223141945.png]]

- To list all your cronjobs, enter the command `crontab -l`.  
- To remove a crontab, you use the command `crontab -r`.
- To create a cronjob as a root user, you enter the command `sudo crontab -e -u root`. Don't forget to use `sudo` !
- To create a cronjob as another user, you enter the command `crontab -e -u user`.

### System Cron Tables

- Linux systems typically scheduled to run many commands during the nonbuisness hours.
- You can list them with : `cat /etc/crontab` :
  ![[Pasted image 20240223142818.png]]
