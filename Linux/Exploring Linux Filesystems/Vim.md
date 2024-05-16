
## Cursor Movement :

![image](vi.png)

## Basic Editing 

### Appending Text

- Chose the place to place your Text, press `i` for insert mode and write your Test down

### Opening a line

- Here are some of the line opening keys in Vi/Vim ( To type before `Insert` ! )

- `i` : insert before the cursor 
- `I` : insert at the beginning of the line
- `o` : open a new line below the current line 
- `O` : open a new line above the current line

### Deleting Text

![image](dd.png)

### Cutting, Copying, and Pasting Text 

- The command `d` dont only delete the Text/Character, it also cuts and it and make it ready for past (yank) for the next time.

![image](yy.png)

- To undo changes press `u` Button

### Search in replace 

#### Searching the entire File 

To search for a word in Vi/Vim, follow these steps:

	1.  Enter normal mode by pressing the Esc key if you're in insert mode.
	2.  Type the command `/` followed by the word you want to search for.
	3.  Press Enter.

### Global Search and Replace

To perform a global search and replace in Vi/Vim, you can use the following command in normal mode:

`:%s/old-word/new-word/g`

The `%` symbol represents all lines in the file, `s` is for substitute, `old-word` is the word you want to replace, `new-word` is the word you want to replace it with, and `g` stands for global, meaning it will replace all occurrences on each line.

For example, if you want to replace all occurrences of "old" with "new" in the entire file, you would type the following:

`:%s/old/new/g`

Press Enter to execute the command. To undo the change, type `u` in normal mode.

- To delete a String with search bar :
	- `:%s/word-to-delete//g`
- To delete for example `/bin/bash` :
	- `:%s/\/usr\/bin//g`
- To search for a word in Vim use `/` and the word after : `/USER`

### Rename the File

- To rename the file within the vim command line : `:saveas newfilename`

### Select all 

- to select all in terminal in vim :

> `:%y+`
