More : [More_Linux (Youtube)](https://www.youtube.com/watch?v=UN1QB5BIvps&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=14)
- [[grep, uniq, Backslash]], [[Linux Commands 3]], [[Linux Commands 4]], [[Linux Commands 5]] 

## System Information:

| Shell Command                             | Specification |
| ----------------------------------------- | ------------- |
| ```whoami```                              |  Display Host Name             |
| ``` id```                                 |  Returns Users Identities            |
| ```unname -a```                           |  Print basic infos about the Operation System             |
| ```pwd```                                 |  returns the Working Directory             |
| ```env```                                 |  shows the Directory of everything ( SSH, Mail, Users ...)             |


## Navigation Stuff:

| Shell Command | Specification           |
| ------------- | ----------------------- |
| ```ls -a ```  | shows also hiding Files |
| ```ls / ```   | lists the root          |
| ```cd / ```   | go to root              |
| ```ls -lt```  | Sort by Time            |
| `| wc -l`     | #count Files                         |

- `wc` :
  ![[Pasted image 20240516123246.png]]
	- `34` : Line Number.
	- `49` : Word count.
	- `335`: Byte Count.
## Add Remove Files/Directory:

| Shell Command                               | Specification                                               |
| ------------------------------------------- | ----------------------------------------------------------- |
| ```touch Text.txt```                        | create a Text File                                          |
| ```cat Text.txt```                          | print Text.txt at the Terminal                              |
| ```makdir New_Folder```                     | make a new_Folder                                           |
| ```rmdir New_Folder```                      | remove New_Folder                                           |
| ```mv Text.txt New_Folder/```               | move to New_Folder                                          |
| ```rm Text.text~ ```                        | remove Text.txt                                             |
| ```mkdir -p Folder_1/Folder_2/Folder_3```   | create multiple Folders in one Folder                       |
| ```rm -r Folder_1```                        | delete everything inside the Folder                         |
| ```cp Folder_1/Text.txt test.txt ```        | make a duplicate of the same file in the same folder        |
| ```grep 81.143.211.90 access.log ```        | grep all infos about the IP-Adress from the access.log file |
| ```echo 'bla bla bla' >> passwords.txt ```  | add the text to the File                                    |
| ```mv Folder_1 -t Folder_2 ```              | move Folder_1 to Folder_2                                   |
| ```realpath >File_Name< or >Folder_Name<``` | Find the Path to the Folder or the File                     |
| ```file >File_Name<```                      | gives you the type of the File                              |
| ```locate  --basename 'File_Name'```        | search for the path to a directory                          |                                            |                                                             |

## Shell Features:

 - Pipes :
	1. ```STDIN = 0 ```or <Text.txt as an input
	1. ```STDOUT = 1``` is the same as >> append or creat a file
	2. ```STDERR = 2``` > create or overwrite a file

## IP Address :

```Shell
ip r                           = show the default gateway
arp -n                         = show all Hosts with IP Address and MAC-Address
arp -e                         = Display the Name of the Hostes, IP and Mac-Addresses

Sudo ss -ulpen  		       = See wich ports are used 

```
  


