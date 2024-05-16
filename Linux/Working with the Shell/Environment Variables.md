- Many variables are set by default in the shell. To display them enter the command `set` :
  ![[Pasted image 20240202195144.png]]


> [!NOTE] Note
> Each Time you enter the shell, the shell loads all the variables from `set` or the **environment variables** 

- Common BASH shell environment are typically executed in the order :
  
```
/etc/profile                 ## set the user (or root)
/etc/profile.d/*             ## set desktop enveronment 
/etc/bashrc                  ## alias for all users (System-wide!)
~/.bashrc                    ## alias for a specific user.
~/.bash_profile              ## set the variables, after login.
~/.bash_login
~/.profile
```

![[Pasted image 20240418131932.png]]

1. The shell looks for the `/etc/profile` and execute it .
2. The shell runs the `.bash_profile`. it basecally says to the shell to find the `.bashrc` file and execute it :
   ![[Pasted image 20240418130110.png]]

2. Execute the `.bash_login`. shell responsible for the login shells.
3. the `.profile` loads the private users variables and configurations.
### PATH

Sample contents of the PATH variable are shown in the following output:
  
```Bash
	[root@server1 ~]# echo $PATH
	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
	[root@server1 ~]#_
```

In this example, if the user had typed the command `ls` at the command prompt and pressed Enter, the shell would have noticed the lack of a `/` character in the pathname and proceeded to search for the file ls in the `/usr/local/sbin` directory, then the `/usr/local/bin` directory, the `/usr/sbin` directory, and then the `/usr/bin` directory before finding the ls executable file. If no ls file is found in any
directory in the `PATH` variable, the shell returns an error message, as shown here with a misspelled
command and any similar command suggestions:

```bash
	[root@server1 ~]# lss
	bash: lss: command not found…
	Similar command is: 'ls'
```


---

#### Common Bash environment variables :

![[Pasted image 20240202195640.png]]

---

### User-Defined Variables

- You can create your own variables with like :
  -  `MYVAR="This is a simple variable"`.
- You can echo the variable with : `echo $MYVAR` .
- Now this variable gonna disappear it you log out as a user, switched to another shell (exited a shell) or rebooted the system.
- To make the system persistent also for other subshells, must run the command ***export** command .* This ensures that all the programs can access the variable.


```bash
[root@server1 ~]# set | grep MYVAR
MYVAR='This is a sample variable'
[root@server1 ~]# env | grep MYVAR
[root@server1 ~]#_
[root@server1 ~]# export MYVAR
[root@server1 ~]# env | grep MYVAR
MYVAR=This is a sample variable
[root@server1 ~]#_
```



> [!NOTE] Note
> - To print the exported variables in your shell, enter the command *printenv* or *env* .
> - To remove the variable from the shell memory, enter the command *unset* or *env -u*


#### Alias

- To add an Alias :
  
```bash
[root@server1 ~]# alias dw="date;who"
[root@server1 ~]# alias | grep dw
alias dw='date;who'
[root@server1 ~]# dw
Tue Aug 7 13:54:43 EDT 2023
root
tty5
2023-08-07 13:31
root
pts/0
2023-08-07 13:38
[root@server1 ~]#_
```

- To remove an Alias :
  - you can enter the command ***unalias** command* .


 