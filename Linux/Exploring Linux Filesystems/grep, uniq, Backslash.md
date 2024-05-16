
## Pipelines :

### The Difference Between `>` and `|` :

- The main difference between `>` and `|` is :
	- `command1 | command2` : Pipes is used to direct the output to another command
	- `command1 > file1` : Pipes the output into another file.

### `Uniq`:

- the most used and usefull command to sort the output is : 
	`$ ls /bin /usr/bin | sort | uniq | less`
- If you want to see the list of duplicate files use `-d`:
	`$ ls /bin /usr/bin | sort | uniq -d | less`

### `grep` :

Here are some common options for the #grep command in Linux:

-   `-i`: ignore case
-   `-v`: invert match
-   `-r`: search recursively in directories (in subdirectory files also)
-   `-l`: only show the names of files containing matches
-   `-n`: show line numbers for matches
-   `-w`: match whole words only
-   `-E`: use extended regular expressions
-   `-o`: show only the matching parts of a line
-   `-c`: count the number of matches in each file
-   `--color`: highlight matches in color.
-  `-e`:  search for multiple patterns 
	- `grep -e apple -e cherry sample.txt` 
		 `apple cherry`

### Backslash Escape Sequences :

![image](bb.png)

- try `\a` : type ``echo -e "This is a test\a"`` this should echo a bell.

