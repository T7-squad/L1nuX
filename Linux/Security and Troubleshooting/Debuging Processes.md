
- One the most used tools to debug a process or software is the `strace` command.
![[Pasted image 20240426195951.png]]

- Install the command with :
  
```bash
  sudo apt install strace
```

## Trace through pid

- You can trace or debugg the program through the pid and output the logs into a text file, with the command :
  
```bash
  sudo strace -p pid -o outfile.txt
```


## Trace a program by executing it

- Or you can trace a program simply by executing it :

```bash
strace program
```

Example :

```bash
strace mousepad
```

![[Pasted image 20240426200625.png]]

The output of the debugging is too long to display !



