## Common Package Manager

- `Package_name` : refers to the actual name of the package
- `file_name` : refers to the name of the file in the package

### Package Search Command

- You can search for your Package in the package repository :
	- Ex: `apt-cache search firefox`
- Youll then get multiple packages related to Firefox, you can select one and install it.

![image](ppp.png)

### Installing Packages from the Repository

- Once you found you wanted package, you can install it with this command.
- Make sure that you always make you package manager updated

![image](pm.png)

### Installing a Package from a Package File

![image](Links_Images/dp.png)

### Removing a Package

![image](pb.png)

### Updating Package

![image](up.png)

### Upgrading a Package from a Package File

![image](ll.png)

### Listing Installed Packages

![image](pc.png)

### Determining Whether a Package Is Installed

![image](ps.png)

### Displaying Information About an Installed Package

![image](pi.png)

### Searching for a package in non-installed Package Manager

- `apt-cache search <package-name>`


### The difference between `apt-get install` and `dpkg --install`

-   `apt-get install` is a front-end to the Advanced Packaging Tool (APT), which is a package management system used for managing packages on Debian-based systems. It automatically handles package dependencies, downloads the necessary packages and their dependencies, and configures them after installation.
    
-   `dpkg --install` is a lower-level command that directly installs a package using the Debian Package Manager (dpkg). It does not handle dependencies, so if a package depends on other packages, they must be installed manually before using `dpkg --install`.

### The Difference between `apt -get update` and `sudo -apt get upgrade` :

-   `apt-get update` is used to download the package information from the configured sources, such as the official package repositories, and update the local cache of available packages. Running `apt-get update` regularly helps ensure that you have the latest package information and prevents issues with installing outdated packages.
-   `apt-get upgrade` is used to upgrade the installed packages to the latest available version. The `apt-get upgrade` command checks the local package cache and the package repositories for upgrades, downloads the updated packages and dependencies, and installs the upgrades. Running `apt-get upgrade` regularly helps keep your system up-to-date with the latest security fixes and bug fixes.
