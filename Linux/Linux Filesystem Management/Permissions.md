
## Linux Files Permission :

### Change Permisssions : 

- Syntaxe :

```Shell
chmode +x 'Filename' or 'Filepath'
```
```Shell
	chmod abc (777) Owner
``` 

| ```drwx-xr-x```   | ```rwx``` : read, write and execute (root) | ```xr``` : execute and read (second Group) | ```x``` Execute (Everyone) |
| ----------------- | ------------------------------------------ | ------------------------------------------- | -------------------------- |
| ```-rw-rw--r--``` | read, write (Owner)                        | read, write (Group)                        | Read (Everyone)                           |


| Octal | File Mode | Comands           |
| ----- | --------- | ----------------- |
| 0     | ---       | do nothing        |
| 1     | --x       | only Execute      |
| 2     | -w-       | write only        |
| 3     | -wx       | write and execute |
| 4     | r--       | read only         |
| 5     | r-x       | read and execute  |
| 6     | rw-       | read and write    |
| 7     | rwx       | read write and execute                  |

## Reading, Writing and Executing

![image](exe.png)

## `chmod` : Change File Mode 

![image](cm.png)

- The Common `chmod` file mode changing : `7 (rwx), 6 (rw-), 5 (r-x), 4 (r--), and 0 (---)`.
- The options for the `chmod` command are:
	-   `u`: user/owner
	-   `g`: group
	-   `o`: others (other users)
	-   `a`: all (equivalent to `ugo`)
	-   `+`: add permissions
	-   `-`: remove permissions
	-   `=`: set exact permissions
	-   `rwx`: read, write, execute permissions
	-   `r`: read permission
	-   `w`: write permission
	-   `x`: execute permission
- The symbolic notation options for the `chmod` command are:
	-   `u+rwx`: add read, write, execute permissions for the user/owner
	-   `g-w`: remove write permission for the group
	-   `o=r`: set read permission for others
	-   `a+x`: add execute permission for all
	-   `+rwx`: add read, write, execute permissions for all
	-   `-rw`: remove read and write permissions for all
	-   `u=rwx`: set user/owner permissions to read, write, execute
	-   `ug+rwx`: add read, write, execute permissions for user/owner and group
	-   `go-rwx`: remove read, write, execute permissions for group and others.

- Here are a few examples of using the `chmod` command in Linux:

	1.  To give read and write permissions to the owner of a file:
		`chmod u+rw file.txt`
	2.  To remove execute permission for group and others on a directory:
		`chmod go-x directory`
	3.  To make a file readable and executable by everyone:
		`chmod a+rx file.sh`
	4.  To make a directory only accessible by its owner:
		`chmod 700 directory`
	5.  To change permissions using numerical values (octal representation):
		`chmod 754 file.txt`


> [!NOTE]
> - To enable a *sticky bit* to a Directory (disabeling the deleting of a directory , like `/tmp` for example) you use the option `-t` :
>   
> ```
>   sudo chmod u+t /temp
> ```


### `umask` 

- The Maximal number of permissions ( in general ) is `777`.
- To calculate the permissions with a `umask` rule of `177` for example is :
  - You muss first know it the file is *executable*. If it, you substart `7` from each number :
    
     - `7-1 = 6` : 6 gives the `rw` permissions.
     - `7-7 = 0` : gives no permissions.
     - `7-7 = 0` : gives also no permissions.

- For a *non executable* file, the umask would look like `022`.
  - The maximum number of permission is this example is : `666`. 
  - Lets find the permissions :
    
    - `6-0 = 6` : 6 gives `rw`.
    - `6-2 = 4` : 4 gives the `r` permission.