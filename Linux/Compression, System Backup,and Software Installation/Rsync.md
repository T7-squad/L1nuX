
### rsync Archive

- Zip the files and archive them in a particular Path :

```bash
rsync -av --compress-choice=zlib  /home/magellan/* /home/magellan/Desktop/backup/ALL_2023_03_26.tar.gz
```

 `/home/magellan/*` : Create an archive for all the Directories/Files in `/home/magellan`
 `/home/magellan/Desktop/backup/ALL_2023_03_26.tar.gz` : Store the Zipped archive as `.tar.gz` in `/home/magellan/Desktop/backup`
 `-a` : Preserve File Permissions. Recursive
 `-v` : Verbose

- synchronize :

```Python
$ rsync -av --delete /Directory1/ /Directory2/
```

The code above will synchronize the contents of Directory1 to Directory2, and leave no differences between the two. If rsync finds that Directory2 has a file that Directory1 does not, it will delete it. If rsync finds a file that has been changed, created, or deleted in Directory1, it will reflect those same changes to Directory2.


#### Exclusion

##### File Exclusion

```bash
rsync -avz --exclude 'FileName' /path/to/source/ /path/to/destination/
```

- `--exclude 'FileName'` : will exclude the filename from the sync

##### Directory Exclusion

```bash
rsync -avz --exclude 'FolderName' /path/to/source/ /path/to/destination/
```

- `--exclude 'FolderName'` : will exclude the filename from the sync

##### Extension Exclusion

```bash
rsync -avz --exclude '*.ext' /path/to/source/ /path/to/destination/
```

##### Write an Exclusion File

- Sometimes you have a lot of file exclusion to deal with. we can allways write them in a text file and implement then to rsync :

Exemple of File Exclusion `exclude-list.txt`:

```Python
FolderName/
FileName
*.ext
```

- Now implement the file text in rsync :

```bash
rsync -avz --exclude-from 'exclude-list.txt' /path/to/source/ /path/to/destination/

```

#### Inclusion

- Sometimes you have some specific Files to archive. You can write the path to them in a text file and implement them in the rsync :

Exemple of File Exclusion `Include-list.txt`:

```Python
FolderName/
FileName
*.ext
```

Then :

```bash
rsync -avz --files-from=/path/to/include.txt /home/magellan/Documents/ /home/magellan/Desktop/backup/Documents/
```

### Rsync with SSH

Lets say you ssh to Termux (Android) via you linux machine and you want to send send FIles over to your LInux Machine. 

If very probable that ssh on termux is runned on another port than port 22.
To verify lunch `nmap localhost` on your termux. after entering `sshd` to run SSH service.
Youll See that the SSH POrt is `8055`
After Cheking the Port number, select you Data to Transfer and add `-L` flag to rsync to not copy LInks to you LInux instead of FIles :

```bash
rsync -avL --compress-choice=zlib -e ssh ./storage/* magellan@Linux_IP:/home/magellan/Desktop/backup/SAMSUNG_2023_03_27.tar.gz
```

This command will send all the storage FIles to the backup directory in LInux Desktop DIrectory !

> [!NOTE] USEFUL
> A very usefull option is the `rsync -av --ignore-existing ...` in rsync to skip existing files.

## rsync—Remote File and Directory Synchronization

### Sync Files /Directories from a Server

- The Syntax for using `rsync` is : `rsync [options] source [destination]`
	- `source` : local file 
	- `destination` : `user@remote-host:remote-file` this option uses SSH Tunnel
- Options :
	- `-a` : This option is for archive; copy all Folders, sub-folders and files 
	- `-v` : verbose
	- `-h` : humain readable
	- `-z` : Compress the files during the transfer
	- `-r` : recursive
-  Ex: 
	- `rsync -avzh /root/music root@192.168.0.141:/root/`
	- Copy the file `/root/music` from localhost to root on `192.168.0.141` in the root directory `/root/`

### Sync Files /Directories from a Server using SSH

- `-e` : specify the remote shell to use, in this case `ssh`
- Exemple :
	- `rsync -avhze ssh /foo user@remote-host:/tmp/`

### Sync Files /Directories Localy

- Exemples : 
	- `rsync -zvh backup.tar.gz /tmp/backups`
	- `rsync -avzh /root/music /tmp/backups`

### Automaticaly Delete source Files after successful transfer

- To do this you add the option `--remove-source-files`
- Exemple :
	- `rsync --remove-source-files -zvh backup.tar.gz root@192.168.0.151:/tmp/backups/`


### Synchronize All files from the Desktop to USB 

- Here's a shell script that you can use to synchronize the files on your desktop with a connected USB device every time you connect it:
	```Bash
	#!/bin/bash

	# Define the source directory (desktop) and destination directory (USB device)
	src_dir=~/Desktop
	dst_dir=/path/to/usb/device
	
	# Check if the USB device is mounted
	if mount | grep "$dst_dir" > /dev/null; then
	  # Synchronize the files using rsync
	  rsync -avzr --delete "$src_dir/" "$dst_dir"
	else
	  # Print a message indicating that the USB device is not connected
	  echo "USB device is not connected."
	fi

```

	- Note: You need to replace "/path/to/usb/device" with the actual path of your USB device.

	- Save the script with a `.sh` extension, for example `sync_usb.sh`, and make it executable using the following command:
		`chmod +x sync_usb.sh`

- Finally, you can run the script manually or set up a udev rule to run it automatically every time you connect your USB device.

- To set up a `udev` rule that runs the shell script every time you connect your USB device, you can create a new `udev` rule as follows:

	1.  Create a new file in the `/etc/udev/rules.d` directory with a `.rules` extension. For example, `/etc/udev/rules.d/99-sync-usb.rules`.
    
	2.  Add the following content to the file, replacing `/path/to/sync_usb.sh` with the actual path of your shell script:
    
		`ACTION=="add", SUBSYSTEM=="block", KERNEL=="sd[a-z][0-9]", RUN+="/bin/bash /path/to/sync_usb.sh"`
	
		This rule matches the `add` event for any block device (representing a storage device) that starts with `sd` followed by a lowercase letter and a digit (such as `sda1` or `sdb2`), and runs the shell script.

3.  Save the file and reload the `udev` rules with the following command:

	`udevadm control --reload-rules`

- Now, every time you connect your USB device, the shell script should be executed automatically. You can verify this by checking the output of the script or by looking at the log files in `/var/log`.

