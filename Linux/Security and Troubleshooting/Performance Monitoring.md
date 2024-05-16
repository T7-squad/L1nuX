## Monitoring Performance with sysstat Utilities

### CPU

- To monitor your CPU, you can use the command `mpstat`.
- Before you do that, make sure you installed the `sysstat` utilities : 
  
```
  sudo apt install sysstat
```

![[Pasted image 20240317145753.png]]


### CPU and Hard Disk

#### iostat

- Another utility is the `iostat` (*input/output statistics* ) measures the flow of informations from and to disk devices (statistics of disk drives).
![[Pasted image 20240317150827.png]]

- Here is an example running  `iostat 2 -m` to display the disk statistics in M
![[Pasted image 20240317151118.png]]

   - `tps` : number of transfer per second.
   - `Mb_wrtn/s` : number of written per second.
   - `MB_dscd/s` ``: discarded per second.

- `iostat -p` display all the Input/Output Statistics of all the Disks (Hardware) :
  ![[Pasted image 20240506115529.png]]

#### ioping

- The `ioping` **works ONLY to test hard drives !**.
- To install the tool :
  
```
  sudo apt-get install ioping
```

- Here are some options :
![[Pasted image 20240317153621.png]]

- Here is one of the use :
![[Pasted image 20240317153709.png]]
### Memory

- To display the memory usage, use the command `vmstat` :
  ![[Pasted image 20240317152424.png]]![[Pasted image 20240317152450.png]]


### ALL of them

- The `sar` command is more powerful then the other tools.
- Here are some options :
  ![[Pasted image 20240317152015.png]]
  ![[Pasted image 20240317152037.png]]

- `sar` can also display Network statistics.
- Type the command to see all the detailed informations about the system : 
  
```bash
  sar -u ALL 1 -A
```

![[Pasted image 20240506120236.png]]

and more !

