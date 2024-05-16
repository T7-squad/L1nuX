- The first process started by the linux kernel is the *init* daemon, which has a PID of *1* and PPID of *0*.
- The Init daemon lunches then other daemons, on of them is the *login daemon* :
  
  ![[Pasted image 20240222165742.png]]


> [!NOTE] Note
> The init daemon is reffered as the Grandfather of all user processes.

- Lets take the command `ps -ef` :
  ![[Pasted image 20240222175836.png]]
  - The Proccess `[kthreadd]` is the Parent PID (PPID of 2) of a lot of child processes; they all have the PPID of 2 (look at the image).
  - The most of other daemons have the PPID of 1, which means that they are all triggered by the init process.

- Common `ps` commands :
  ![[Pasted image 20240222184047.png]]

- To show all the processes that are running on the terminal : `ps -x` :
  ![[Pasted image 20240222184307.png]]

- One of the coolest is the *pstree*. Its helpful to understand PPID and PID :
  ![[Pasted image 20240222184421.png]]

- If you want to search for the PID of certain processes like for example flameshot that is been used by a user, you can utilize the tool *pgrep*.
  ![[Pasted image 20240222192734.png]]
  ![[Pasted image 20240222192802.png]]

- Recursively, you can check which process is running on a given port. you do that with the *lsof* command :
  - `lsof -p 1399` : display the process that is running on that PID.  
- You also can use the command *killall*, which kills the process and child process .
  - `killall -9 sample` : will completely kill the program sample.  
- `killall -9 sample` is the same as `killall -SIGKILL sample`.


- Display the process ID of the a giving process (name) 
  
  
```bash
  pidof sshd
```

![[Pasted image 20240506171303.png]]
## run a program with modified priority

#### nice

- The `nice` Command ==lunches== a program/command with modified scheduling priority.
- Info :
  ![[Pasted image 20240415214445.png]]

- To give a high priority to a program, lets say firefox, use the `-n -20` :
  
```
  sudo nice -n -20 firefox
```

#### renice

- `renice` command is used to ==adjust== the priority of an existing proccess.
- Info :
  ![[Pasted image 20240415215023.png]]

- To adjust a high priority (`-20`) priority of a PID of `123`  :
  
```bash
  renice -n -10 -p 123
```


## Display the Kernel Parameters (CPU,Memory)

- To do that run the command :
  
```bash
  sudo ulimit -a
```

![[Pasted image 20240419125103.png]]

