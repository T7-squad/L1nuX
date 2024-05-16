
# Invoking the Shell

## Command Line Options

Here are some common options in the `bash` shell, along with examples to help illustrate how they work:

-   `-c`: This option allows you to run a single command in a new shell. For example:

	`bash -c "ls -l"`
	
	This will run the `ls -l` command in a new `bash` shell and display the output.

-   `-i`: This option starts an interactive shell, allowing you to type commands and see the results. For example:

	`bash -i`
	
	This will start an interactive `bash` shell, where you can type commands and see the results.

	![image](fg.png)
	

-   `-n`: This option reads the commands from a script file but does not execute them. For example:

	`bash -n script.sh`
	
	This will read the commands in `script.sh` but not execute them. This can be useful for checking a script for syntax errors before running it.
	- Exemple: say you want to debugg the script `my_script.sh` .
	- To see if any error accured befor running the script, you run the `bash -n my_script.sh` on the terminal
	- If there is some error, youll get them displayed on the terminal.
	- Exemple : `./my_script.sh: line 2: syntax error: unexpected end of file`

-   `-r`: This option starts a restricted shell, which restricts certain features of the shell that could be used to compromise system security. For example:

	`bash -r`
	
	This will start a restricted `bash` shell, where certain features are restricted.

	![image](12.png)
	
	

-   `-x`: This option turns on debugging, causing the shell to display each command as it is executed. For example:

	`bash -x script.sh`
	
	This will run the commands in `script.sh` and display each command as it is executed. This can be useful for debugging scripts.

	![image](123.png)

-   `-v`: This option turns on verbose mode, causing the shell to display each command before it is executed. For example:

	`bash -v script.sh`
	
	This will run the commands in `script.sh` and display each command before it is executed. This can be useful for understanding the flow of a script.

- `-o` : gives all the shell options :

![image](11.png)

## Command Exit Status

- The shell automatically retrieve the value if the shell exists.
- This means : True or False
- Exit status codes are used to check the success or failure of a command and take appropriate action in a shell script. For example, you can use the exit status code to determine whether to continue processing the script or to exit and display an error message.

- Here are some common exit status codes:

	-   0: Successful completion
	-   1: General error
	-   2: Misuse of shell builtins
	-   126: Command invoked cannot execute
	-   127: Command not found
	-   128: Invalid argument to exit
	-   130: Script terminated by Control-C
	-   255: Exit status out of range

- The exit status of a command can be accessed in a shell script using the `$?` special variable.

![image](sc.png)

# Special Files

- The `/etc/profile` file is a system-wide configuration file for the Bourne Again SHell (Bash) in Linux. It contains environment settings and startup commands that are executed whenever a user logs into the system.
- Here is a simple example of what the `/etc/profile` file might look like:

```Shell
#!/bin/bash

# Set the PATH variable
export PATH=$PATH:/usr/local/bin

# Set up aliases
alias ls='ls --color=auto'
alias ll='ls -l'

# Set the default shell prompt
PS1='[\u@\h \W]\$ '

# Add any other startup commands here

```

### File Name Metacharacter 

Sure, here are some examples for each filename metacharacter:

-   `*`: matches any number of characters

```SHELL
# List all files in the current directory that have an extension of .txt
ls *txt
# Delete all files in the current directory that have an extension of .bak
rm *bak

```

-   `?`: matches any single character

```Shell
# List all files in the current directory that have a name of exactly 3 characters
ls ???

# Delete all files in the current directory that have a name of exactly 3 characters
rm ???

# Remove all the files that starts with file
rm file?*

```

-   `[...]`: matches any one of the characters enclosed within the brackets

```SHELL
# List all files in the current directory that have a name that starts with a, b, or c
ls [abc]*

# Delete all files in the current directory that have a name that starts with a, b, or c
rm [abc]*
```

-   `[!...]`: matches any character not enclosed within the brackets

```SHELL
# List all files in the current directory that have a name that does not start with a, b, or c
ls [!abc]*

# Delete all files in the current directory that have a name that does not start with a, b, or c
rm [!abc]*

```

-   `{...}`: matches one of the comma-separated patterns within the curly braces

```SHELL
# List all files in the current directory that have an extension of .txt or .md
ls *{txt,md}

# Delete all files in the current directory that have an extension of .txt or .md
rm *{txt,md}
```

- `@` : This metacharacter suppose to match every string inclueding white space.
	- The `@` metacharacter in bash matches exactly one instance of a pattern. This means that if you use `@` in a globbing pattern, it will only match a single character in the filename :
	 ```Shell
	file1.txt
	file2.txt
	file3.txt
	```
	- In this exemple the `file@.txt` will match the first file number 
		`file1.txt`

### Brace Expension 

- `echo pre{1..10..2}post` : Will create an incrementation by 2 of the numbers betwee 1 and 10 between the characters pre and post :
	- `pre1post pre3post pre5post pre7post pre9post`

![image](3.png)

![image](112.png)

### Escape Sequeces

- These are the three contexts in which Bash recognizes and interprets special escape sequences. The `$'...'` quoted string, `echo -e` and `printf %b` arguments, and `printf` format strings allow you to include escape sequences in your scripts and commands to control the format and output of text.
- `echo -e` : Enable to Bash to read the escape sequences from your input. Here are an exemple for using/non using `echo -e` :

![image](vb.png)
- Here some exemples of the Sequences :

![image](ad.png)

### Quoting
cd
![image](tb.png)

### Command Forms

![image](cf.png)

![image](es.png)

### Redirection Forms

![image](rf.png)

#### Simple Redirection

| Command      | Explanation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |     |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| cmd > file   | Send output of cmd to file (overwrite).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |     |
| cmd >> file  | Send output of cmd to file (append).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |     |
| cmd < file   | Take input for cmd from file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |     |
| cmd << text  | The contents of the shell script up to a line identical to text become the standard input for cmd (text can be stored in a shell variable). This command form is sometimes called a here document. Input is typed at the keyboard or in the shell program. Commands that typically use this syntax include cat, ex, and sed. (If <<- is used, leading tabs are stripped from the contents of the here document, and the tabs are ignored when comparing input with the end-ofinput text marker.) If any part of text is quoted, the input is passed through verbatim. Otherwise, the contents are processed for variable, command, and arithmetic substitutions. |     |
| cmd <<< word | Supply text of word, with trailing newline, as input to cmd.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |     |
| cmd <> file  | Open le for reading and writing on the standard input. The contents are not destroyed.1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |     |
| cmd > file   | Send output of cmd to le (overwrite), even if the shell’s noclobber option is set.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |     |

##### `stderr` , `stdin` , `stdout` :

`stdin`, `stdout`, and `stderr` are standard streams in the Unix shell and are used to represent the standard input, standard output, and standard error of a process, respectively.

1.  `stdin` (Standard Input): This is the default input stream for a process, which typically reads input from the keyboard. A program can read from `stdin` to receive data as input.
	- `$ cat < input_file.txt` : redirect the output of the file as input for `cat`
    
2.  `stdout` (Standard Output): This is the default output stream for a process, which typically writes output to the terminal. A program can write to `stdout` to display information to the user.
	- `$ ls > output_file.txt` : Redirect the output of the command `ls` to the file.
	
3.  `stderr` (Standard Error): This is the default error output stream for a process, which typically writes error messages to the terminal. A program can write to `stderr` to report error conditions or to provide additional information about its operation.
	- `ls non_existent_directory 2> error.log` : redirect the errors to `error.log`

##### How `stderr` , `stdin` , `stdout` could be helpful ?

`stdin` and `stdout` can be incredibly useful in a variety of different ways in the Unix shell:

1.  Input and Output Redirection: By default, input is read from `stdin` and output is written to `stdout`. However, you can redirect `stdin` and `stdout` to files or other processes to manipulate the input and output of commands. For example, you can redirect the output of a command to a file to save its results for later use, or you can redirect the input of a command from a file to provide it with data to process.
    
2.  Piping: By connecting the `stdout` of one command to the `stdin` of another command, you can create a pipeline of commands that work together to accomplish a specific task. This allows you to chain together multiple commands to perform complex operations.
    
3.  Scripting: By redirecting `stdin` and `stdout` in a script, you can automate repetitive tasks and make your scripts more flexible. For example, you can write a script that reads data from a file and processes it, or a script that writes the output of a command to a file for later analysis.
    
4.  Debugging: By redirecting `stderr` to a file, you can capture error messages and diagnostic information generated by commands and scripts. This can be incredibly helpful in debugging problems, as you can inspect the error messages to identify the cause of the problem.
    
5.  Processing Data: By using `stdin` and `stdout` to read and write data, you can process data in a variety of different ways. For example, you can filter data to include only certain elements, sort data, or extract specific information from a large dataset.

#### Redirection using File descriptors

![image](ruf.png)

![image](mr.png)


# Functions

- In bash, functions are blocks of code that can be executed whenever you call their name. Functions can accept parameters and return values.
- The basic syntax is : 

```sHELL
function_name () {
  # function body's code
} [redirections]
```

Or

```Shell
name () { 
	commands 
	return
}
```

-The parameters of the function is passed simply after invoking the function `$1` : parameter 1, `$2` : parameter 2 ...

Exemple 1 :

```Shell
add () {
  local sum=$(($1 + $2))
  echo "The sum of $1 and $2 is $sum"
}
```

The add function is called in the command line like this :

```Shell
add 10 20
```

Exemple 2 :

- The Bash-Script :

![image](sh.png)
- The Input :

![image](sh1.png)

# Variables

## Variable Assignment

- ! Variables are assigned WITHOUT whitespaces between them !
- `firstname=Arnold lastname=Robbins numkids=4 numpets=1` are exemples of variables assignement.
- If you declare a number variable for exemple `i=5-0` , bash will take the number and treat it like a string ! if you dont want to, just add `declare -i` :

![image](dec.png)

- `+=` : is used to append a variable. Exemple :
	
	![image](tm.png)

## Variable Substitution

![image](vs.png)
![image](vs1.png)
![image](vs2.png)
![image](vs3.png)

## Build in Shell variables

Here is a list of some of the most commonly used built-in shell variables in Bash:

1.  `$0` - The name of the shell or script.
2.  `$1-$9` - The first 9 arguments passed to the script.
3.  `$#` - The number of arguments passed to the script.
4.  `$@` - All the arguments passed to the script.
5.  `$*` - All the arguments passed to the script as a single string.
6.  `$? `- The exit status of the last executed command.

8.  `$!` - The process ID of the last background command.
9.  `$PS1` - The primary prompt string.
10.  `$PS2` - The secondary prompt string.
11.  `$IFS` - The internal field separator that separates command line arguments.
12.  `$HOME` - The home directory of the current user.
13.  `$PWD` - The current working directory.
14.  `BASH`: The full path of the bash executable.
15.  `BASH_VERSION`: The version number of the current bash shell.
16.  `HOME`: The home directory of the current user.
17.  `HOSTNAME`: The hostname of the system.
18.  `IFS`: The internal field separator, used to split lines into fields.
19.  `PS1`: The primary prompt string.
20.  `PS2`: The secondary prompt string.
21.  `PWD`: The current working directory.
22.  `SHELL`: The full path of the shell being used.
23.  `PATH`: A colon-separated list of directories that the shell searches for commands.
24.  `UID`: The numeric user ID of the current user.
25.  `LINENO`: The current line number in the script.
26.  `CDPATH`: A colon-separated list of directories that the cd command searches for directories.
27.  `OLDPWD`: The previous working directory.
28.  `PPID`: The process ID of the parent process.
29.  `RANDOM`: A random number between 0 and 32767.

## Arrays

- The Arrays in Bash are starting with 0 !
- To set up an array use : `( Array )`
	- `List = (1 2 3 4 5 6)`
	- This an array for numbers between 1 and 6
- To reference an Array use `${...}`
- To reference the position of an array use `[...]`
	- `${List[0]} = 1`
	- `${List[2]} = 3`
![image](ay.png)

### Array Substitutions

![image.png](as.png)

![image](as1.png)

