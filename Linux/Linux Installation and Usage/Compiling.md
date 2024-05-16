# Compiling the Source Code into Programs


> [!Important]
> - To install all the ==Tools== needed to compile a source code, you need to run the command :
>   
> ```bash
> ## For Debian :
> 
>   sudo apt-get install build-essential
> 
> ## More Specific :
> 
>  apt-get install build-essential linux-source bc kmod cpio flex libncurses5 dev libelf-dev libssl-dev dwarves bison
> 
> ## For Centos :
> 
>   sudo yum groupinstall "development tools"
> ```
> 


- You do that by *curl* or *wget* the GitHub repository .
- After cd to it, you'll find a `Readme.md` and a `Install` file, that a look into them, because they will give me informations about how to install the program.
- The first step to do is to run `./configure`, this check system requirements and create a list of what to compile inside a File called `MakeFile`.
- Next, you type the `make` command, which looks for the `MakeFile`, looks the informations in it and compile the source code into binary program using the appropriate compiler (in Linux we use the *GNU C Complier* or `gcc` ).
- After compiling the source code into a binary, you need to run the `make install` to install the binary in your system.
- To delete the compiled binary run the command `make uninstall`.

# Compiling the Kernel

- Lets try to compile the kernel manually.
- First download the Kernel from the Kernel [Webpage](www.kernel.org) ![[Pasted image 20240409151334.png]]

- After decompressing the Kernel, you will get a folder like this (with the kernel version) :
  ![[Pasted image 20240409151723.png]]

- `cd` to the new Directory and entre the command `make help` :
  ![[Pasted image 20240409151852.png]]
  
   - You will get all the informations about the `make` command.

### 1. clean

- First thing to do is to clean :
  ![[Pasted image 20240409152135.png]]

- To do that run the command :
  
```bash
  make clean
```

- After that you can also run the command :
  
```bash
  make mrproper
```

