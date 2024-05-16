
## tty and Terminal

- To Display Messages for users on `tty` before they connect to the Console, you can add some line in the `/etc/issue`.
- Here is an example of `/etc/issue` :
  ![[Pasted image 20240423150409.png]]

- After doing the changes, *reboot* the system.
- To change the message, you need to change the entries in the `/etc/issue` :
  ![[Pasted image 20240423152036.png]]

- More in https://www.computernetworkingnotes.com/linux-tutorials/the-etc-motd-and-etc-issue-files-explained.html.
- You can add this to your fish shell by copying the lines into a file `greeting.txt` and edit the `config.fish` file :
   ![[Pasted image 20240423153610.png]]

## SSH

- To display informations when the Client connects to the server like For example this Message :
  ![[Pasted image 20240425141209.png]]

- You need to edit the `/etc/motd` in the server side :
  ![[Pasted image 20240425141251.png]]

- Save & Exit. And now you have it !

- You can make this message appears before the users enter the Username and Password.
  - You do that by editing the `/etc/sshd/ssh_config`, and commenting out `Banner   /etc/issue.net`:
    ![[Pasted image 20240425170859.png]]

  - You edit the `/etc/issue.net` :
    
    ![[Pasted image 20240425171029.png]]
    
  - After that you need to restart your SSH Server.
  - Now if you want to log into the server using SSH, you will get the Message you wrote :
    ![[Pasted image 20240425171129.png]]


    