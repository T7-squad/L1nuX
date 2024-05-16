
## Create an attribute

- The command `chattr` changes the file attribute on a filesystem level.
- The Attributes could be :
  
  - *Immutable (`i`)* : Prevent any modification to the file, encluding renaming, editing or deleting. ==!! This is also valable for the root user !!==.
    
```bash
    sudo chattr +i hallo.txt
```

  - *Append-only (`a`)* : ==You can only add lines at the end of the File !==. Existing file cannot been modified or deleted :
    ![[Pasted image 20240426163917.png]]
    
```bash
    sudo chattr +a hallo.txt
```


## Delete an attribute

- To delete an Attribute, use the command :
  
```bash
  sudo chattr - <option> hallo.txt
```

## List attribute

- To list an attribute, use the command :

```bash
lsattr hallo.txt
```

![[Pasted image 20240426164240.png]]

