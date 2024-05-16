
## Symbolic Links

- A symbolic Link is a Link to a File or a directory that can be created inside or outside the main Storage.
- lets say you want to move your Game outside of your main hard drive and copy it in your external hard drive but still can access it, as if it was on the local hard drive :  
  - Move the diablo directory to the External Hard drive :
    ![[Pasted image 20231212205521.png]]
- create a symbolic link of the Games directory in the external hard drive in the local working directory with the command `ln -s ~/ltb/Games/diablo diablo`.
- Now you have created a symbolic link to your Game :
  ![[Pasted image 20231212210251.png]]

- If you lunch the diablo binary the binary will be lunched 

