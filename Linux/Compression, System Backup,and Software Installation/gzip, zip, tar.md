### `gzip` compressor 

> [!NOTE]
> - `gzip` CANNOT zip directories !!!

- The `gzip` command in Linux has several options that can be used to control its behavior. Here are some common options and examples of how to use them:

1.  Compress a file:
	`gzip file.txt`

2.  Decompress a file:
	`gzip -d file.txt.gz`

3.  Compress multiple files:
	`gzip file1.txt file2.txt`

4.  Compress and keep the original file:
	`gzip -c file.txt > file.txt.gz`

5.  Decompress and display the contents of a file:
	`gzip -dc file.txt.gz`

6.  Compress a file with a specified level of compression (1-9):
	`gzip -9 file.txt`

7.  Show the compression ratio for a file:
	`gzip -l file.txt.gz`

8.  Compress a file to a specified file name:
	`gzip -c file.txt > compressed_file.gz`

9.  List the contents of a compressed archive:
	`gzip -l compressed_file.gz`

![[Pasted image 20240228141656.png]]

### `tar` Archive Tool

`tar` archives are commonly used for backup and file distribution purposes, as they allow you to easily transfer multiple files as a single unit and extract the contents at a later time. The `tar` command provides options for compressing and decompressing the archive file, making it an efficient tool for storing and transmitting large files.

Here are a few common uses of `tar`:

1.  Create a tar archive:
	`tar -cvf archive.tar file1.txt file2.txt`

2.  Extract the contents of a tar archive:
	`tar -xvf archive.tar`

3.  Compress a tar archive with gzip:
	`tar -zcvf archive.tar.gz file1.txt file2.txt`

4.  Extract a tar archive compressed with gzip:
	`tar -zxvf archive.tar.gz`

5.  List the contents of a tar archive:
	`tar -tvf archive.tar`

6. Create a `.tar.gz` archive :
	`tar -zcvf archive.tar.gz file/file_directory`
	With path:
	`tar -czvf /path/to/my_archive.tar.gz /path/to/my_folder`


- These are just a few examples of how to use `tar` in Linux. You can find more information and options for using `tar` by running `man tar` in the terminal.

#### Create a Backup with `tar`

To create a backup of files using `tar` in Linux, you can follow these steps:

1.  Change to the directory that contains the files you want to backup. For example:
	`cd /path/to/directory`

2.  Create a tar archive of the files in the current directory.(`-f`: for File, `-c`: create, `-v`: verbose) For example:
	`tar -cvf backup.tar *`

3.  Verify the contents of the tar archive (`-t` : list ). For example:
	`tar -tvf backup.tar`

4.  (Optional) Compress the tar archive to save disk space. For example:
	`gzip backup.tar`

- This will create a compressed tar archive of the files in the current directory, named `backup.tar.gz`. You can restore the contents of the archive by decompressing it and then extracting the files using the `tar` command. For example:
	`gzip -d backup.tar.gz tar -xvf backup.tar`

- Note: You can also use the `-z` option with `tar` to create a compressed archive directly, without the need for a separate step to compress the file. For example:
	`tar -zcvf backup.tar.gz *`

- /This will create a gzip-compressed tar archive named `backup.tar.gz`.


> [!NOTE] Update a tar Backup !
> - To update your tar backup you run the command `tar -uvf /path/to/backup.tar -C /path/to/newer_files .`


### `zip` Packages and File Compressor

- Zip is a Commpression and archeving Tool, its atmost in use in Windows
- Here is a list of commonly used options for the `zip` utility in Linux, along with examples for each option:

1.  `-r`: Recursively zip all files and directories, including subdirectories. Example: `zip -r archive.zip directory_to_compress/`
    
2.  `-9`: Use maximum compression level. Example: `zip -9 archive.zip file_to_compress`
    
3.  `-m`: Move the original files into the archive, instead of copying them. Example: `zip -m archive.zip file_to_compress`
    
4.  `-j`: Store only the file names in the archive, not the full path. Example: `zip -j archive.zip file_to_compress`
    
5.  `-t`: Display the contents of the archive. Example: `zip -t archive.zip`
    
6.  `-u`: Update the archive with newer versions of files. Example: `zip -u archive.zip file_to_compress`
    
7.  `-T`: Test the archive to verify that all files are readable. Example: `zip -T archive.zip`
