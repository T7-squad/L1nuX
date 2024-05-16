# Chapitre 26 : Top/Down Design

### Local Variables :

Here is an exemple of the use of local variables :

![image](lv.png)
And here is the output :

![image](lv1.png)

### Function and More :

- Here is a script that verifies if an input an @hotmail valid is :

![image](fm.png)

![image](vali.png)

- To check if a giving command has any ouput we use the option `-n` in Bash :

```Shell
output=$(cut -d'/' -f 8- file.txt)
if [ -n "$output" ]; then
    echo "$output"
fi

```

### Using `test` :

- The `test` expression doest atmost the same functionality as `if`. 
- The syntax is : `test expression` or `[ expression ]`
- `[ expression ]` is valued to be either true or false.
- `test expression` returns 0 for True, and 1 for Flase.
- Here are some exemples for the test synonym `[ ]` :

	-   `-e`: Tests if a file exists. For example, `[ -e /etc/passwd ]` will return true if the file "/etc/passwd" exists.
	-   `-f`: Tests if a file is a regular file. For example, `[ -f /etc/passwd ]` will return true if the file "/etc/passwd" is a regular file.
	-   `-d`: Tests if a file is a directory. For example, `[ -d /etc ]` will return true if the file "/etc" is a directory.
	-   `-r`: Tests if a file is readable. For example, `[ -r /etc/passwd ]` will return true if the file "/etc/passwd" is readable.
	-   `-w`: Tests if a file is writable. For example, `[ -w /etc/passwd ]` will return true if the file "/etc/passwd" is writable.
	-   `-x`: Tests if a file is executable. For example, `[ -x /bin/bash ]` will return true if the file "/bin/bash" is executable.
	-   `-z`: Tests if a string is empty. For example, `[ -z "$myvar" ]` will return true if the variable "myvar" is empty. ( !can be used to check if the input was successfull !)
		```Shell
		#!/bin/bash
		# Prompt the user to enter a word
		read -p "Please enter a word: " word
		# Check if the word is empty
		if [ -z "$word" ]; then
		    echo "No word was entered."
		else
		    echo "The word you entered is: $word"
		fi
		```
	-   `-n`: Tests if a string is not empty. For example, `[ -n "$myvar" ]` will return true if the variable "myvar" is not empty.
	-   `==`: Tests if two strings are equal. For example, `[ "$myvar" == "hello" ]` will return true if the variable "myvar" is equal to "hello".
	-   `!=`: Tests if two strings are not equal. For example, `[ "$myvar" != "world" ]` will return true if the variable "myvar" is not equal to "world".

![image](test.png)

### Integer Expressions

- Here some integer expressions :

![image](ie.png)

### A More Modern Version of test

- In the recent versions of bash the `test` is remplaced with the syntax `[[ expression ]]` 
- `[ string =~ regex ]` returns True if the string matches the regex.

![image](rege.png)

### `read`—Read Values from Standard Input

- The `read` is used to read one line input from the user.
 	
 	> The syntax is `read [-options] [variable...]`


1.  `-p` (prompt): This option is used to display a prompt message to the user before reading input. For example:
    
    ```shell
    read -p "Enter your name: " name 
    echo "Hello, $name!"`
```
     

2.  `-s` (silent): This option is used to read input without displaying it on the screen. This can be useful for password prompts. For example:
    ```shell
    read -s -p "Enter your password: " password 
    `echo "Your password is: $password"`
```

    
3.  `-n` (characters): This option is used to read a specific number of characters from input. For example:
    
    ```Shell
    read -n 4 -p "Enter four letters: " letters 
    echo "You entered: $letters"
    ```
    
4.  `-t` (timeout): This option is used to set a timeout period for reading input. If the user does not input anything within the specified time, the command will exit. For example:
    
    ```Shell
    read -t 5 -p "Enter something within 5 seconds: " input 
    if [ -z "$input" ]; then
         echo "Timed out" 
         else     
         echo "You entered: $input" 
         fi
    ```
    
5.  `-r` (raw): This option is used to read input without interpreting backslashes as escape characters. This can be useful for reading file paths or other strings with special characters. For example:
    
    ```shell
    read -r -p "Enter a file path: " path 
    echo "The file path is: $path"
    ```
    

### `Menus` :

- Here is an exemple of a Menu :

```Shell
	Please Select: 
	1. Display System Information 
	2. Display Disk Space 
	3. Display Home Space Utilization 
	0. Quit 
	Enter selection [0-3] >

```

- Here is the script :

![image](mm.png)
![image](mm1.png)


# Chapitre 29 : Flow Control (While/Until)

### `While` loop :

- The syntax of the `While` loop :

> `while commands; do commands; done`

- Like if, while evaluates the exit status of a list of commands. As long as the exit status is 0, it performs the commands inside the loop.

![image](sleep.png)

### `until` loop

- The `until` loop in Bash is similar to the `while` loop, but it repeats a block of code until a condition is true, rather than as long as the condition is true.

```Shell
until condition
do
    # code block to repeat
done
```

- Ex :

```Shell
#!/bin/bash

# Prompt the user for a positive integer
until [ $num -gt 0 ]
do
    read -p "Enter a positive integer: " num
done

echo "The number you entered is $num."

```

In this Exemple, the code is executed until the user enters a positive number.


# Chapitre 31 : Branching with case

### `case` :

- The basic syntax for `case` :

```Shell
case expression in
    pattern1)
        command1
        ;;
    pattern2)
        command2
        ;;
    pattern3)
        command3
        ;;
    *)    ## for other patterns
        default_command
        ;;
esac

```

- Ex 1 :

	- This Exemple Code checks for the file-type :

```Shell
read -p "Enter a file path: " file
case $file in
    *.txt) echo "$file is a text file.";;
    *.pdf) echo "$file is a PDF file.";;
    *.png|*.jpg|*.jpeg) echo "$file is an image file.";;
    *) echo "$file is of unknown type.";;
esac
```

- Ex 2 :

```Shell
#!/bin/bash

echo "Enter a number between 1 and 3"
read num

case $num in
    1)
        echo "You entered one."
        ;;
    2)
        echo "You entered two."
        ;;
    3)
        echo "You entered three."
        ;;
    *)
        echo "You did not enter a number between 1 and 3."
        ;;
esac

```


# Chapitre 32 : Positional parameter

![image](abc.png)

### Determining the number of arguments

![image](arg.png)

