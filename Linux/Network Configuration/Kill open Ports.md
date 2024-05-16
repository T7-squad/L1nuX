
## Kill Open Ports from Nmap

- First run an nmap scan on your system, select the ports that open.
- After selecting the ports open on your machine type the command `sudo lsof -i:PORT` and youll get infos about the the softwares that are using this port.
- Exemple : I found the open Ports 18083 and 57621 on my machine :

![image](lsof.png)

- After discovering the open ports, you can kill them with `kill` command : `sudo kill PID` 


> [!Important]
> - ==OR==, you can use the `netstat` command with the option `--program` :
>   
> ```
>   netstat -tuna --program
> ```
> 
> ![[Pasted image 20240413183712.png]]
> 
> This command will display the open Ports with there coresponding Programs !
