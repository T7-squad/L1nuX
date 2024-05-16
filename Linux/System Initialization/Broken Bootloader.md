
## Check the Hard-drive (root)

- Enter the Boot command line
- Type `ls` to check for your Hard-disks:
  ![[Pasted image 20240409170754.png]]
- check every one of them until you find your Root directory :
  ![[Pasted image 20240409170826.png]]

- Now that you found the root directory, you need to set the Path as a variable :
  
```bash
  set root=(hd0,gpt2)
```

- Control your changes with `set` :
  ![[Pasted image 20240409171018.png]]


## Set the path to the Kernel

- Next, you 