

1. One effective way to run a process (software) in the background, is lunching the process and entering the `Ctrl+Z` after that enter `bg` this will bring the process to the background. 
2. To run a job on the background, you can enter the *&* after the command or program you want to run.
- Example : lets say you want to run the program *okular* on the background. you run the command `okular &` :
  ![[Pasted image 20240222200834.png]]
- Now inter `jobs` to see the program PID in the background :
  ![[Pasted image 20240222200926.png]]
- To bring the program from the Background, you type `fg %1` :
   ![[Pasted image 20240222201053.png]]
- Or just kill the program with `kill -9 %(JobId)`.
