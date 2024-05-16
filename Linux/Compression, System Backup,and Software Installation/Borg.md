### Borg Achive :

- There are multiple Tools on LInux to create backup FIles on a USB , External DIsk or on your machine
- One of this tools is the `Borg Backup` 
- More about the Borg Backup in the [Arch Wiki](https://wiki.archlinux.org/title/Borg_backup)
- Visit Youtube [Borg Packup](https://www.youtube.com/watch?v=q_OdTOuvP4A&t=1060s)
- Vist Borg Official WEbsite [Borg](https://borgbackup.readthedocs.io/en/stable/usage/extract.html)


#### Install Borg Backup :

- To install Borg : `sudo pacman -S borg`. This will install the tool and all the dependencies.

#### Creating repositories

```Python
$ borg init --encryption=none /path/to/repo
```

- `init` will initiate a repository on the path `/path/to/repo`
- the option `--encryption=none` set no file encryption.

#### Creating archives

- To create an archive of the `archivable-dir` directory with the hostname of the source machine and the current date:

```Python
$ borg create /path/to/repo::{hostname}-{now:%Y-%m-%d} archivable-dir
```

- Borg supports extensive inclusion and exclusion options. To exclude `.iso` files from the archive:

```python
$ borg create --progress --stats --exclude '*.iso' /path/to/repo::archive-name
```

- `--progress` : shows you the progress of the archive
- `--stats` : print statistics for the created archive

- Borg supports also compression (zstd is recommended). use `-C` option for compression.

	- Zstandard (`Zstd`): A relatively new compression algorithm that provides a good balance between compression ratio and speed. It's becoming increasingly popular among backup tools. The available compression levels are:
	    -   1: Fastest compression, lowest compression ratio.
	    -   3: Medium compression, medium compression ratio.
	    -   6: Slower compression, higher compression ratio.
	    -   9: Slowest compression, highest compression ratio.
	- Exemples :
	```Python
	$ borg create -C zstd,3 /mnt/backups::documents-zstd-c3-2022-03-23 /home/user/Documents
	```

![image](borg.png)

#### List The Archive

- After running the archive command, you want to list all the Backups that you made .
- To do this run the command : 
```Python
borg list Path/to/archive
```
- After that you can select you archive file and look at all the douments that have been archived .
![image](borgl.png)
List of Archives:
```Python
$ borg list /path/to/repo
Monday                               Mon, 2016-02-15 19:15:11
repo                                 Mon, 2016-02-15 19:26:54
root-2016-02-15                      Mon, 2016-02-15 19:36:29
newname                              Mon, 2016-02-15 19:50:19
...
```

Select an Archive Name `root-2016-02-15 `  :
```Python
$ borg list /path/to/repo::root-2016-02-15
drwxr-xr-x root   root          0 Mon, 2016-02-15 17:44:27 .
drwxrwxr-x root   root          0 Mon, 2016-02-15 19:04:49 bin
-rwxr-xr-x root   root    1029624 Thu, 2014-11-13 00:08:51 bin/bash
lrwxrwxrwx root   root          0 Fri, 2015-03-27 20:24:26 bin/bzcmp -> bzdiff
-rwxr-xr-x root   root       2140 Fri, 2015-03-27 20:24:22 bin/bzdiff
```

![image](listb.png)

#### Restoring from an archive

- To restore the COMPLETE archive and list the files while extracting :

```Python
$ borg extract --list /path/to/repo::archive-name path/to/restore
```

- Extract the ONLY "src" directory from the archive and list the files while extracting :

```Python
$ borg extract --progress --strip-components 2 /home/magellan/Desktop/backup::2023-03-25 /home/magellan/Downloads
```

- `--strip-components 2` : means delete the home and magellan directory and extract only Downloads directory.


#### Deleting an archive 

- Delete a single backup archive (Archivename = Monday):

```Python
$ borg delete /path/to/repo::Monday
```

- Delete all archives whose names contain "-2012-"

```Python
$ borg delete --glob-archives '*-2012-*' /path/to/repo
```

#### Pruning archives

- To keep only the last 7 daily archives, the last four weekly archives, and the last three monthly archives:

```PYTHON
$ borg prune --keep-daily=7 --keep-weekly=4 --keep-monthly=3 /path/to/repo
```

#### Borg with SSH

- You can send the backup to another machine using SSH.

Create :
```Python
$ borg create ssh://backupuser@remoteIP:/mnt/backups::documents-2022-03-23 /home/user/Documents
```

- `-backupuser` : is the hostname on the remote machine
- `/mnt/backups::documents-2022-03-23` : is the destination path on the remote machine, where you want to store your backups
- `/home/user/Documents` : is the directory to backup to the remote machine (local)

