
1. ```htop``` : Task Manager

2. ```tail /etc/passwd``` : and the Terminal shows you all Users in your interface. Ex :

	```
	dave:x:1000:1000:dave,,,:/home/dave:/bin/false
     --> dave       : Username
     --> x          : Password
     --> 1000:1000  : User ID : User Default ID
     --> dave,,,    : Information about the user
     -->/home/dave  : Home directory
     -->/bin/flase  : default Shell ``` 
 
3. ```tail /etc/group``` : display all the groups in your PC 
4. add a User :
```Shell
	useradd -m -d /home/>User_Name -s /bin/bash >User_Name<
```


5. ```tail /etc/shadow```: to see if the Password is set ( if you see :!: that means that the Password is not set yet )
6. ```ls -l /home``` : see if the new User is there.
7. ```passwd 'NewUser'``` : **set a password to the New user**.
8. ```userdel 'NewUse'``` : **delete the new user**
9. ```groupdel -f 'NewUser'``` : Delete complitely a User
10. ```su user2``` : move to user2

## Change Username :

```Shell  
usermod -l <new_name> /home/<new_name> <old_name>
```

- For this command to work you have to connect as another user and go to root and then change the username.


## Add User to the Sudoers :

- to run as root  !

```SHELL
sudo adduser user1 sudo = Add a User to the sudoers
```


## Add User/Delete :

```SHELL  
sudo adduser <Username> = Add a User
```

```SHELL
sudo passwd <Username> = Create a Password for the User.
```

```SHELL
sudo userdel <Username> = Delete a User
```

## `useradd` :

- Here are some common options for the `useradd` command:

	-   `-c`: specify a comment or description for the new user
	-   `-d`: specify the home directory for the new user
	-   `-e`: specify an expiration date for the new user account
	-   `-g`: specify the primary group for the new user
	-   `-G`: specify additional groups for the new user
	-   `-M`: do not create a home directory for the new user
	-   `-m`: create a home directory for the new user
	-   `-p`: specify an encrypted password for the new user
	-   `-u`: specify a user ID (UID) for the new user
	-   `-s`: specify a shell for the new user
	
- Here some Exemples for each option :

	- -   `useradd -c "John Doe" johndoe`: creates a new user with the comment "John Doe" and username "johndoe".
	-   `useradd -d /home/johndoe -m johndoe`: creates a new user with a home directory at `/home/johndoe` and username "johndoe". The `-m` option creates the home directory if it does not already exist.
	-   `useradd -g newgroup johndoe`: creates a new user with the primary group "newgroup" and username "johndoe".
	-   `useradd -G group1,group2 johndoe`: creates a new user and adds it to additional groups "group1" and "group2", and username "johndoe".
	-   `useradd -M johndoe`: creates a new user without a home directory, and username "johndoe".
	-   `useradd -p $6$SaltedHash johndoe`: creates a new user with the encrypted password "$6$SaltedHash" and username "johndoe".
	-   `useradd -u 1000 johndoe`: creates a new user with the UID 1000 and username "johndoe".
	-   `useradd -s /bin/bash johndoe`: creates a new user with the default shell `/bin/bash` and username "johndoe".

## `groupadd` :

- Here are some common options for the `groupadd` command:
	
	 -   `-f`: force the creation of the group, even if a group with the same name already exists
	-   `-g`: specify a GID (group ID) for the new group
	-   `-r`: create a system group, which is reserved for system use

- Here are some examples of using the `groupadd` command:

	-   `groupadd sales`: creates a new group named "sales".
	-   `groupadd -g 5000 sales`: creates a new group named "sales" with the GID 5000.
	-   `groupadd -r ops`: creates a new system group named "ops".
	-   `groupadd -f ops`: creates a new group named "ops" even if a group with the same name already exists.
- There are some common specifications for Group IDs (GIDs) in Unix-like operating systems, but these specifications can vary between systems and distributions. Here are some common specifications:

	-   System accounts and system groups: These are typically assigned a low GID in the range 0-499. For example, the `root` user is often assigned the GID 0.
	-   User accounts: These are typically assigned a GID in the range 1000-60000. Some distributions reserve the range 1000-29999 for user accounts and assign the range 30000-60000 for other groups. 
	-   Dynamic groups: These are typically assigned a GID in the range 500-999 or 300000-999999.

- A Group ID (GID) is a unique identifier assigned to a group in a Unix-like operating system. The GID is used to identify the group in the system and to control access to files and resources that are owned by the group. In most systems, each group has a unique GID and each user can belong to multiple groups. When a user creates a file or a directory, it is assigned to the group of which the user is a member. Other users who are members of the same group can access and modify the file or directory, subject to the permissions set by the owner.
- To list all groups : `cat /etc/group`
- To delete a group : `sudo groupdel <username>`

## `usermod` :

- `usermod` is a command line tool in Unix-like operating systems that allows an administrator to modify the properties of an existing user account. With the `usermod` command, you can change various aspects of a user account, such as the user name, home directory, default shell, and other properties. The `usermod` command can also be used to modify the group membership of a user, add or remove the user from supplementary groups, and change the expiration date of the user account.

- Here are some common options for the `usermod` command:

	-   `-l`: change the login name of the user
	-   `-d`: change the home directory of the user
	-   `-u`: change the UID (user ID) of the user
	-   `-g`: change the primary group of the user
	-   `-G`: add or remove the user from supplementary groups
	-   `-s`: change the default shell of the user
	-   `-a`: append the user to the supplementary groups listed
	-   `-e`: change the expiration date of the user account

## `userdel` :

- When using the `userdel` command to delete a user in Linux, you have the following options:

	-  `-f` or `--force`: forcibly remove the user, even if the user is still logged in.  
	-   `-r` or `--remove`: remove the user's home directory and mail spool.  
	-   `-R` or `--root`: specify an alternate root directory.

- To delete complitely the user and his home directory use : ``userdel -r <username>``

## `chown` :

- Here are the options for the `chown` command in Linux, with examples:

	1.  `-R`: Recursively changes ownership of files and directories. Example: `chown -R username:group directory_name`
	2.  `-v`: Verbosely shows the files that have been changed. Example: `chown -v username:group file_name` 
	3.  `--no-dereference`: Changes the ownership of symbolic links rather than the files they point to. Example: `chown --no-dereference username:group symlink_name` 
	4.  `--from=current_owner:current_group`: Changes ownership only for files that are currently owned by `current_owner` and `current_group`. Example: `chown --from=old_owner:old_group username:group file_name`

- More :

![image](ch.png)


