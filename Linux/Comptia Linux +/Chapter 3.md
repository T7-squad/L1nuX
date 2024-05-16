# 137.

- Answer is *A*.
	![[Pasted image 20240516123246.png]]
	- `34` : Line Number.
	- `49` : Word count.
	- `335`: Byte Count.

# 139.

- Answer is *D*.
- ![[Pasted image 20240516123818.png]]
- More in [[Docker installation]].

# 142.

- Answer is *C*.
- Sending a command to the STDOUT means that the results of the command has to be displayed on the screen.
- In this Example the command `cat /etc/passwd | tee pass` will display the file on the terminal and pipe the containt into a file.

# 146.

- Answer is *B*.
- *xargs* enables you to run command from an input command :
  ![[Pasted image 20240516131415.png]]

- To delete a file you can use the command :
  
```bash
  find . -type f -name "text.txt" | xargs /usr/bin/rm -rf
```

