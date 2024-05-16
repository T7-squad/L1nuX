## find :

+ Ex1:

```Shell
find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03



Thibovski@htb[/htb]$ find <location> <options>

```


| **Option**         | **Description**                                                                           |
| ------------------ | ----------------------------------------------------------------------------------------- |
| ```-type f```      | - Hereby, we define the type of the searched object. In this case, 'f' stands for 'file'. |
| ```-type d```      | - d for Directory                                                                         |
| ```name.*config``` | with '-name', we indicate the name of the File.                                           |
| ```-user root```   | search in the root directory                                                              |

Ex2 :

-  What is the name of the config file that has been created after 2020-03-03 and is smaller than 28k but larger than 25k?

```Shell
find / -name *.conf -size +25k -size -28k -newermt 2020-03-03 2>/dev/null
```


- 
  ```Shell

-   find . -name flag1.txt: find the file named “flag1.txt” in the current directory
-   find /home -name flag1.txt: find the file names “flag1.txt” in the /home directory
-   find / -type d -name config: find the directory named config under “/”
-   find / -type f -perm 0777: find files with the 777 permissions (files readable, writable, and executable by all users)
-   find / -perm a=x: find executable files
-   find /home -user frank: find all files for user “frank” under “/home”
-   find / -mtime 10: find files that were modified in the last 10 days
-   find / -atime 10: find files that were accessed in the last 10 day
-   find / -cmin -60: find files changed within the last hour (60 minutes)
-   find / -amin -60: find files accesses within the last hour (60 minutes)
-   find / -size 50M: find files with a 50 MB size

```
  