## /etc/shadow

1. Lets brake down the root password in the `/etc/shadow` :
  
  `root:$6$PpnHJ3rl/6OouP9G$cKhZbi96b9UxfkHOt4ahHvrW9V73vg9BLqiEHKFOfcIAvktJ.fP2V9RoFkxgz6tcW9LlEspV2FN1j4g5vy90M1:17745:0:99999:7:::`


- The root, as many other users, passwords are stored in the `/etc/shadow` in this Form :
  
  `name:password:lastchange:min:max:warn:disable1:disable2:`
  
  - `min` : represents the minimum of days the user must wait until he change his password.*min=0 days*
  - `max` : represents the maximum of days the user can login using the same password. *max=99999 days* since since January 1, 1970.
  - `7` : you are warned 7 days before your password needs to be changed.


> [!Important]
> - All this Rules for Users :
>   
>   
>   - Rules for the Password.
>   - Setting the Home Directory.
>   - Groups.
>   - Encrytion method.
>   - Login Restrictions ...
>     
>     are set in the `/etc/login.defs` directory :
>     ![[Pasted image 20240425143443.png]]
>   ![[Pasted image 20240425143533.png]]
> ## Creating User Accounts

- More in [[User and Group Management]]
  
### Useradd

- To create a user accounts on linux use the command `useradd <name of the username>`.
- When you implement the command `useradd` the  script located in `/etc/login.defs` will automatically add the user with the UIID, Group number, hash his password ... 
- In the Directory `/etc/default/useradd` you will find the configuration for adding a new user, like expiration date, user ID ... :
  ![[Pasted image 20240226184721.png]]

- `useradd` commands:
  ![[Pasted image 20240226185148.png]]

- After adding the new user, the `skel` will also be added :
  
  ![[Pasted image 20240227165047.png]]

- The `/etc/skel` is combination of the `.bash_logout`, `.bashrc`, `.profile`.

## Modifying User Accounts

- To modify the most informations about a user use the command `usermod`.
- For example change the username from user1 to user2, you can add the `-l` option to `usermod` :
  
  `usermod -l user2 user1`

- Common options for `usermod` :
![[Pasted image 20240226185753.png]]

- You can add informations about a user (Phone number, Organisation) by entering the command `chfn` :
  ![[Pasted image 20240226193942.png]]


> [!NOTE] Note
> - The only thing that the `usermod` cannot change is the password expiration (min,max,warn).
> - To change this information use the command `chage`


- For example, to specify that `Bini` that is, the user `binigr` must wait 2 days before changing his password after receiving a new password, as well as to specify that his password expires every 50 days with 7 days of warning prior to expiration, you can use the following options to the `chage` command:
  
```
  [root@server1 ~]# chage -m 2 –M 50 –W 7 binigr
```

> [!NOTE]
> - Before using `chage` you need to install `emboss` with `sudo apt install emboss`.
> - Delete the expiration Date with: `chage -E -1 username`.



- To change the home directory of a user to another :

```  
usermod -l <new_name> -d /home/<new_name> -m <old_name> 
```

![[Pasted image 20240227175826.png]]

> [!NOTE]
> - Set a command on the username (add a real username) :
>   
> ```
>   sudo usermod -c "John Smith" john
> ```
> 

### Example 1

- Rename the user from bonzo to ZEP :
  
```
  useradd -l ZEP bonzo
```

- Change the UID of the user ZEP to 666 :

```
usermod -u 666 ZEP
```
![[Pasted image 20240227172959.png]]

- Set the expiration day to *14 day* :

```
usermod -f 14 ZEP
```

- Set the Day of the expiration to *11.03.2024* :

```
usermod -e "11/03/2024"
```

- ZEP has to wait at least 2 days before making the password changes :

```
chage –m 2 ZEP
```

- ZEP should change his password every 40 days :

```
chage –M 40 ZEP
```

- Send a warning to change the password every 5 days :

```
chage –W 5 bozo
```

- Results :

![[Pasted image 20240227173828.png]]

### Listing Informations about the user (expiration date ...)

- To display the Informations about the user, such as *password expiration date* and more, enter the command :
  
```bash
  chage -l username
```
![[Pasted image 20240419122427.png]]


### Locking an Account

- To *lock* a user `user1` from logging you enter the command :
  
```
  usermod -L user1
```

- This will add the `!` character at the beginning of the encrypted password in `/etc/shadow`.

![[Pasted image 20240227174508.png]]

- To *unlock* the user :
  
```
  usermod -U user1
```

![[Pasted image 20240227174743.png]]


## Deleting User Accounts


- To delete a user account use the command `userdel` and add the name of the user. This will remove the entries in `/etc/shadow` and `/etc/passwd`.
- To delete the corresponding home directory, you can add the option `-r`, this will delete the home directory and all of its content.

## Managing Groups

- To `add` a group use the command `groupadd`.
- To add the group named `group1` and assign to it the GID of `492` :
  
```
  groupadd -g 492 group1
```

- To add `maryj` to the `group1` use the command :
  
```
usermod –aG group1 maryj
```

- Verify with :

```bash
[root@server1 ~]# tail -1 /etc/group
group1:x:492:maryj
```

- You can use the command `groupmod` command to modify the group name and GID, and use the `groupdel` to remove groups from the system.
- To list all the groups use the command `groups`, to see the GID for each group run the command `id` :
  
```bash
[root@server1 ~]# groups
root bin daemon sys adm disk wheel
[root@server1 ~]# id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),
4(adm),6(disk),10(wheel)
[root@server1 ~]#_
```

## Listing all the Groups

- You can use the command :
  
```
  getent
```

To Retrieve all the groups from (`/etc/nsswitch.conf`).
![[Pasted image 20240418162224.png]]


## Run a command as another user

- `pkexec` enables you to run a command as another user :
  ![[Pasted image 20240425152637.png]]

