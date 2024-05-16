# Chapitre 19 : Regular Expressions

## `grep` --search trough text

A regular expression (regex) is a pattern that describes a set of strings. In Linux, regular expressions can be used in many utilities, such as `grep`, `sed`, and `awk`, to search and manipulate text.

Here are some examples of using regex in Linux with explanations:

1.  `^A`: Matches a line that starts with the letter 'A'.
	- ```$ echo "Apple" | grep "^A"
			Apple
			
2.  `[0-9]`: Matches any digit.
	- `$ echo "123456" | grep "[0-9]"
			``123456``
			
3.  `[a-zA-Z0-9]`: Matches any letter (uppercase or lowercase) or digit.
	- `$ echo "abcABC123" | grep "[a-zA-Z0-9]"
			`abcABC123`
			
4.  `.`: Matches any character (except a newline).
	- `$ echo "Hello World" | grep "."`
	
5.  `*`: Matches zero or more of the preceding character or group.
	- `$ echo "Mississippi" | grep "i.*i"`
	
6.  `.*`: Matches zero or more of any character (including newlines).
	- `$ echo -e "Hello\nWorld" | grep ".*" Hello World`
			`Hello`
			`World`
			
7.  `[a-z]+`: Matches one or more lowercase letters.
	- `$ echo "hello" | grep "[a-z]+"`
	
8.  `[A-Z][a-z]*`: Matches a capital letter followed by zero or more lowercase letters.
	- `$ echo "Hello" | grep "[A-Z][a-z]*"`
	
9.  `^[0-9]+$`: Matches one or more digits at the beginning and end of a line.
	- `$ echo "12345" | grep "^[0-9]+$"`
	
10.  `^[a-zA-Z0-9]+$`: Matches a string that consists of only letters (upper or lowercase) and digits.
	- `$ echo "abcABC123" | grep "^[a-zA-Z0-9]+$"`
	
11.  `^[A-Z][a-zA-Z0-9]*`: Matches a string that starts with a capital letter, followed by zero or more letters (upper or lowercase) or digits.
	- `$ echo "HelloWorld" | grep "^[A-Z][a-zA-Z0-9]*"`
	
12.  `[a-zA-Z]+\.[a-zA-Z]{2,3}$`: Matches a string that consists of one or more letters (upper or lowercase), followed by a dot and two or three more letters. This could be used to match file extensions, like `.txt` or `.pdf`. 
	- `$ echo "file.txt" | grep "[a-zA-Z]+\.[a-zA-Z]{2,3}$"`
	
13.  `[0-9]{3}-[0-9]{2}-[0-9]{4}`: Matches a string that consists of three digits, a hyphen, two more digits, another hyphen, and four more digits. This could be used to match a Social Security number in the format `123-45-6789`.
	- `$ echo "123-45-6789" | grep "[0-9]{3}-[0-9]{2}-[0-9]{4}" 123-45-6789`
	
14.  `http(s)?://[A-Za-z0-9./]+`: Matches a string that represents a URL that starts with "http" or "https", followed by a colon, two slashes, and one or more characters (letters, digits, dots, or slashes)
	- `$ echo "https://www.google.com" | grep "http(s)?://[A-Za-z0-9./]+"
	    ``https://www.google.com
	    
15. `\.` :  matches the lateral dot character `.`

16. `\b` : is a word boudery, which markes the position between a word and a non word (space)

17. `\w*` : matches zeros or more characters

- More Exemples :

1.  `.` (dot) - Matches any single character except a newline character. Example: "a.c" would match "abc", "a1c", etc.
    
2.  `*` - Matches zero or more of the preceding character or group. Example: "a.*c" would match "ac", "abc", "a1c2c", etc.
    
3.  `+` - Matches one or more of the preceding character or group. Example: "a.+c" would match "abc", "a1c2c", etc., but not "ac".
    
4.  `?` - Matches zero or one of the preceding character or group. Example: "a.?c" would match "ac" and "abc", but not "a1c2c".
    
5.  `[ ]` (square brackets) - Matches any single character within the brackets. Example: "a[0123]c" would match "a0c", "a1c", "a2c", and "a3c".
    
6.  `[^ ]` (negated square brackets) - Matches any single character not within the brackets. Example: "a[^0123]c" would match any string that starts with "a", ends with "c", and doesn't have "0", "1", "2", or "3" in between.
    
7.  `^` (caret) - Matches the start of a line. Example: "^a" would match any line that starts with "a".
    
8.  `$` (dollar sign) - Matches the end of a line. Example: "a$" would match any line that ends with "a".
    
9.  `\w` - Matches any word character (letters, digits, and underscore). Example: "\w+" would match "word", "Word1", etc.
    
10.  `\d` - Matches any digit. Example: "\d+" would match "123", "456", etc.
- Exemples :
	- `/^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$` search for US-Phone numbers like (286) 254-2860
	- `\(` : correspond to `(`
	- `\)` : correspond to `)`
	- `\/` : correspond to `/`
	- `locate --regex 'bin/(bz|gz|zip)'` : locate /bin/bz OR /bin/gz OR /bin/zip


# Chapitre 20 : Text Processing

## Slicing and Dicing

### `cut` Command 

Here are the options for the 'cut' command in Linux:

1.  `-b`: Specifies the byte positions for the cut. Example: `cut -b 1,3 file.txt` (will extract the first and third byte from the file.txt)
    
2.  `-c`: Specifies the character positions for the cut. Example: `cut -c 1,3 file.txt` (will extract the first and third character from the file.txt)
    
3.  `-d`: Specifies the delimiter to be used for the cut. The default delimiter is TAB. Example: `cut -d ":" -f 1 file.txt `(will extract the first field using the delimiter `:` from the file.txt)
    
4.  `-f`: Specifies the field numbers to be extracted. Example: `cut -d "," -f 1,3 file.txt` (will extract the first and third field using the delimiter `,` from the file.txt)
	- `\t` : is the space deliminator.
    
5.  `--output-delimiter`: Specifies the output delimiter for the cut. Example: `cut -d "," -f 1 --output-delimiter ":" file.txt `(will extract the first field using the delimiter `,` from the file.txt and the output will be separated by `:`)
    
6.  `--complement`: Specifies the complement of the fields to be extracted. Example: `cut -d "," -f 2 --complement file.txt` (will extract all the fields except the second field from the file.txt using the delimiter `,`)

### `join` command 

- `join` command takes 2 files as input and merge the lines from each file that have MATCHING values in a specified field.
- By default is the whitespace as deliminator, but this could be changed with the option `-t`.
 ![image](at.png)

### `tr`—Transliterate or Delete Characters

- The `tr` (translate or truncate) command in Linux is used to translate or delete characters from standard input and write the results to standard output. It's often used for operations such as converting uppercase letters to lowercase, deleting specific characters, or replacing specific characters with others.

- Here are some of the common options available for the `tr` command:

1.  `-d`: Deletes the specified characters.

Example: `echo "HELLO WORLD" | tr -d "HLOW"` will output "E RD".

2.  `-s`: Squeezes repeating characters into a single instance.

Example: `echo "HELLO WORLD" | tr -s " "` will output "HELLO WORLD".

3.  `[:lower:]`: Translates uppercase characters to lowercase.

Example: `echo "HELLO WORLD" | tr "[:upper:]" "[:lower:]"` will output "hello world".

4.  `[:digit:]`: Deletes all non-digit characters.

Example: `echo "Hello 123 World" | tr -d "[:alpha:]"` will output "123".

5.  `[:space:]`: Deletes all space characters.

Example: `echo "Hello World" | tr -d "[:space:]"` will output "HelloWorld".

6. `echo "Hello World" | tr 'e' 'a'` : remplace `e` with `a` .

- Note: The examples use the pipe (`|`) symbol to send the output of one command as the input to the `tr` command. The characters within the square brackets (e.g. `[:upper:]`) are called character classes and are used to specify sets of characters to be translated or deleted.
- To delete multiple Character with `tr` : 
	- Lets say you want to delete `:%` and `/` :
		- The syntax would be : `tr -d ":%/"`
	- Lets say you want to delete both `[[:upper:]]`  and `:`
		- `tr -d ':[[:upper:]]'`

### `sed` Stream Editor for Filtering and Transforming Text

- `sed` (Stream EDitor) is a powerful text processing tool in Linux that can perform various transformations on text streams. `sed` reads the input file, line by line, and makes changes to each line as specified by a set of editing commands.

- Here are some common options for `sed`:

1.  `-n`: This option tells `sed` to only print the lines that match the specified pattern. By default, `sed` prints every line, even if it has not been changed.
	
	`sed -n '/Hello/p' input.txt`
	This will print only the lines that contain the string "Hello" in the file `input.txt`.

2.  `s/old/new/`: This is the substitution command in `sed`, which replaces the first occurrence of `old` with `new` on each line.
	
	`sed 's/Hello/Hi/' input.txt`
	This will replace the first occurrence of the string "Hello" with "Hi" on each line of the file `input.txt`.

3.  `g`: This flag is added after the substitution command to replace all occurrences of the `old` string with the `new` string on each line.

	`sed 's/Hello/Hi/g' input.txt`
	This will replace all occurrences of the string "Hello" with "Hi" on each line of the file `input.txt`.

4.  `d`: This command deletes the lines that match the specified pattern.

	`sed '/Hello/d' input.txt`
	This will delete all lines that contain the string "Hello" in the file `input.txt`.

5. `p` : Is used to print the pattern space (buffer). Exemples :

	- `sed -n '/word/p' file.txt` : Print only the line of the file that contains `world`
	- `sed -n '/[0-9]\{3\}/p' file.txt` : Print the line that contain this set of characters
	- `sed 's/word/new_word/p' file.txt` : print the changed line
	- `sed -n '=' file.txt | sed -n 'N;s/\n/ /p'` : Print the line and the line number
6. `=` : Print the current line number

### `nl` -number of lines

- `nl` is a unix utility that numbers the lines of files. it add line numbers to the output text.
- `nl file.txt` : Print the text in file.txt with numbers for each line

	![image](fk.png)