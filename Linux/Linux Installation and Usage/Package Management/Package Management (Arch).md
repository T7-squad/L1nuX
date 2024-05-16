
## Updating / Upgrating 

- To update and upgrade packages in Arch Linux, you can use the `pacman` package manager.
- you can run the following command as root or using sudo:

> `sudo pacman -Syu`

- `-S` : Synchronize the package database.
	- `-y` : option is used to refresh the package database before upgrading packages
	- `-u` : option is used to upgrade packages to their latest versions.

- Sometimes if you run an update, its more often that pacman will reinstall packages. To prevent pacman from reinstalling the packages, add the option `--needed` : `sudo -Syu blackarch --needed`. This will skip reinstalling already existing packages

### Silence Up/grade 

- To silence the Update/Upgrade function; if you dont want to answer by Yes/No to every Package you want to install, use the option `--noconfirm`

> `pacman -Syu --noconfirm`

### Resolving dependencies 

- Somethimes you cant remove a packages because its depents on other packages.
- To solve this issue run : `sudo pacman -Rsc lbd` on the package. In this exemple i used the package name `lbd`
	-   `-R`: This option tells `pacman` to remove a package from the system.
	-   `-s`: This option tells `pacman` to remove the package and its dependencies that are not required by any other installed package. This is useful to avoid leaving unused dependencies on the system.
	-   `-c`: This option tells `pacman` to remove packages that were installed as dependencies of another package, but are no longer required by any other package. This is useful to clean up the system and remove unused packages.

### Problem with keys :

- Some times when you run `sudo pacman -Syu` you get keys errors.
1. `pacman -Sy archlinux-keyring` to update the keys.
 2. `pacman -Syu` to retry the update .

- Or  `sudo pacman-key --populate archlinux` and the `pacman -Sy archlinux-keyring`


### Search for a Package in Pacman

- To search for a package in pacman package repository type : `sudo pacman -Ss` to see all the packages in the packman repository.
- To search if a package is in the repository type : `sudo pacman -Ss >package-name<`
- To see if this package is allready installed in you linux type : `sudo pacman -Qs >Package-name<`

### Installing the Black Arch repository 

- To install the blacl arch repository enter the command :

> `sudo pacman -S blackarch`

- This will install all the tools for arch linux
- To skip allready installed packages use the option `--needed` : `sudo pacman -S blackarch --needed`
-   `pacman -S <package>`: Install a package.
-   `pacman -R <package>`: Remove a package.
-   `pacman -Syu`: Update the package database and upgrade all installed packages to the latest version.
-   `pacman -Ss <keywords>`: Search for a package using keywords.
-   `pacman -Qi <package>`: Show detailed information about a package.
-   `pacman -Ql <package>`: List all files installed by a package.
-   `pacman -Qo <file>`: Find which package a file belongs to.

### Finding the Packages in the Path of your Local Machine 

- Sometimes you wanna find the Path to a specific package and youll face some diffeculty.
- To print out the Path to all of your packages use the command : 
  
  > `pacman -Qlq >Package-Name<
    
-   `-Q`: specifies that we want to query the package database.
-   `-l`: tells pacman to list all files that are installed by the package(s).
-   `-q`: this option only prints the package names, without version numbers or other information.


Broadcast message from root@virtaulbix on pts/0
The system will power off now !