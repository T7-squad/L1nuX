
- There are two types of packages :
  - *Pre Compiled packages :* This packages can be found in the *Debian Package Manager* or in a sandbox format like *Snap* , *Flatpak* and *AppImage*.
  - *Packages that mus be compiled* : This packages are found in the *GitHUb* repository .

# Working with the Debian Package Manager (DPM)

## `dpkg-reconfigure`

- The `dpkg-reconfigure` enables you to enable the changes you made to a package.
- Example :
  - ``sudo dpkg-reconfigure keyboard-configuration`` : enables you to reconfigure the keyboard layout.
  - ``sudo dpkg-reconfigure tzdata`` : enables you to reconfigure the timezone.
  - `sudo dpkg-reconfigure xserver-xorg` : reconfigure the xorg-server.
  - ``sudo dpkg-reconfigure apache2`` : reset the Apache server to default settings.

## Fixing Broken packages

- Lets say you encounter this Error :
  
```bash
The following packages have unmet dependencies:
 auditd : Depends: libaudit1 (= 1:3.1.2-2) but 1:3.1.2-2.1 is to be installed
          Depends: libauparse0 (= 1:3.1.2-2) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.

```


Lets brake the down the Error :

- The Software depends on a software `libaudit1` with the new version `1:3.1.2-2`. Now the Linux system only has the old version of `libaudit1` which is the version `1:3.1.2-2.1` which is *Incompatible* with the software.
- The same is for the other software.

- To solve this issue, update the Linux repository with `sudo apt update`.
- Install the new versions needed by the Software manually :
  
```bash
  sudo apt install libaudit1=1:3.1.2-2 libauparse0=1:3.1.2-2
```
![[Pasted image 20240328144547.png]]

## dpkg

- Here are some useful options used in dpkg :

![[Pasted image 20240302083320.png]]

- `dpkg -l` or `dpkg-query -l` : list all installed packages :
  ![[Pasted image 20240302084339.png]]

- `dpkg -S` search for the files related to a specific package. Example `dpkg -S dgm3` :
  ![[Pasted image 20240302084631.png]]

- `dpkg -s` gives you informations like the version, status, location of the conf files, dependencies and installation status. Example `dpkg -s virtualbox` :
  ![[Pasted image 20240302085300.png]]

## Search for packages in the Repo

- To search for a specific package in the Repo, you use the `apt-cache` command :
  ![[Pasted image 20240302092342.png]]

- `apt-cache search bluetooth` : will search for packages related to Bluetooth :
  ![[Pasted image 20240302092510.png]]

- `apt-cache show bluez` : will show all the informations about the package bluez :
  ![[Pasted image 20240302101551.png]]


## Understanding Shared Libraries

- While installing a package, the `apt` installer checks for dependencies for the package, load and install them.

> [!Important]
> - With the `dpkg` manager, things are not simple. You have to list all the dependencies for a package and install them separately, before you install any package. The same works for compiling a program !

- If a dependency is removed from a package (during update or manually removing the package) this package will stop working.
- Shared libraries are located in `/lib`, `/lib64`, `/usr/lib`, or `/usr/lib64`.
- To identify which shared library a program has, use the command `ldd` :
  ![[Pasted image 20240302105059.png]]
  ![[Pasted image 20240302105307.png]]

- If a shared library is missing, you will see *not found* in the output of the *ldd* command.

## Working with Sandboxed Applications

- Sandboxed application solves the dependency issues while installing packages.
- Sandboxed applications ensures that each package has an unique copy of his dependency.
- This solves the dependency issues.

### Flatpak

- Add the Flatpak repository : `flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo ; flatpak remote-modify --enable flathub`.
- `flatpak remote-ls` : list all applications present in the Flatpak repo.
- More :
![[Pasted image 20240302112002.png]]

### Snap

- Install Snap with `sudo apt install snapd`.
- More :
![[Pasted image 20240302112224.png]]

