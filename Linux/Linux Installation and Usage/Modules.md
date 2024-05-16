### Understanding the Linux Device Drivers

- For the a driver to work well on Linux, the Linux kernel must have correct device driver.
- At the boot process in which the kernel is loaded into memory, the kernel search and detect devices within the System.
- The Linux Kernel looks for this important devices in the `/dev` directory.
- This devices have a device driver loaded into the Linux kernel and loaded as *Module*.
- The *Modules* ends with `.ko` (Kernel Object) and are typically stored in the `/lib/modules/kernelversion` or `/lib/modules/kernelversion` Directory.
- The module can be manually loaded into the kernel with the command ***insmod** command* or ***modprobe** command* .
- To see the list of modules that are loaded into linux kernel you can use the command ***lsmod** command* .
  
  ![[Pasted image 20240215202700.png]]


- To display the information about a particular module use the command ***modinfo** command* :
  ![[Pasted image 20240215202512.png]]
  ![[Pasted image 20240215202615.png]]

- To remove a module use the command ***rmmod** command* .
- If the Linux kernel was unable to load the modules at boot time, its search then the configuration files.
  - The linux kernel load This configuration files from the `/etc/modprobe.conf` , `/etc/modprobe.conf` or `/etc/modules` text files or under the directory `/etc/modprobe.d` or `/etc/modules-load.d`.
  - New software are added to this directory to be loaded by the kernel at boot time.
  - Example :
    ![[Pasted image 20240218122249.png]]

- Adding a new packages will add the modules to a subfolder withing `/etc/modules/$(uname -r)` or `/lib/modules/$(uname -r)` : 
  ![[Pasted image 20240218123240.png]]

- After installing a new Module by adding the linux device driver package, you should run ***depmod** command*, to upload the module dependency.

