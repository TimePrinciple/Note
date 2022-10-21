### `U`

Unlike `u` which undoes the last change. `U` undoes all the changes made on the last line that was edited.

### `CTRL-]` & `CTRL-O`

- Jump to a subject under the cursor.

- Jump back (repeat to go further back).

### Make bars, stars and high-light visible

The bars and stars are usually hidden with the <u>conceal</u> lfeature. They also use <u>hl-Ignore</u>, using the same color for the text as the background. Make them visible with:
```
:set conceallevel=0
:hi link HelpBar Normal
:hi link HelpStar Normal
```
### Create a `init.vim` file.

To create an empty vimrc:
```
:call mkdir(stdpath('config'),'p')
:exe 'edit' stdpath('config').'/init.vim'
:write
```
### `ZZ`

Same as `:wq` which writes the file and exit.

### `:e!`

Reloads the original version of the file.

### Search all help pages

- `:helpgerp /<content/>`: Search in all help pages (and also of any installed plugins). This takes to the first match.

- `:cnext`: This go to the next one.

- `:copen`: This open the quickfix window displays all available matches.

### Word movement

"x" marks the starting point of the cursor:

- `w` command moves to the start of the next word:
```
	This is a line with example text
	  x-->-->->----------------->
	   w  w  w    3w
```
- `b` command moves backward to the start of the previous word:
```
	This is a line with example text
	<----<--<-<---------<--x
	   b   b b    2b      b
```
- `e` command moves to the next end of a word, `ge` moves to the previous end of a word:
```
	This is a line with example text
	   <----<----x---->------------>
	   2ge   ge     e       2e
```
- `iskeyword`:

It is also possible to move by white-space separated WORDs. This is not a word in the normal sense, that's why the uppercase is used. The commands for moving by WORDs are also uppercase, as this figure shows:
```
		   ge      b		  w								e
		   <-     <-		 --->					       --->
	This is-a line, with special/separated/words (and some more).
	   <----- <-----		 -------------------->	       ----->
	     gE      B					 W						 E
```
### Move to the start or end of a line. 

- `$` command moves the cursor to the end of a line.

- `^` command moves to the first non-blank character of the line.

- `0` command moves to the very first character of the line.
```
			  ^
	     <-----------x
	.....This is a line with example text
	<----------------x   x-------------->
			0				   $
```
(the "....." indicates blanks here)

### Move to a character

- `f<char>` command ("f" stands for "Find") searches to the right in the line for the single character **\<char\>**, the cursor will be positioned over that character:
```
	To err is human. To really foul up you need a computer.
	---------->-------------->
	    fh		 fy
			  -------------------->
					   3fl
```
- `F<char>` command searches to the left:
```
	To err is human. To really foul up you need a computer.
			  <---------------------
					    Fh
```
- `t<char>` command ("t" stands for "To") works like the "f" command, except it stops one character before the searched character. 
```
	To err is human. To really foul up you need a computer.
			   <------------  ------------>
					Th			   tn
```
These four commands can be repeated with ";". "," repeats in the other direction. The cursor is never moved to another line. Not even when the sentence continues.

### Move to parenthesis

`%` command: moves to the matching parenthesis. If the cursor is on a "(" it will move to the matching ")". If it's on a ")" it will move to the matching "(".
```
			    %
			 <----->
	if (a == (b * c) / d)
	   <---------------->
			    %
```
This also works for [] and {} pairs. (This can be defined with the 'matchpairs' option.)

When the cursor is not on a useful character, "%" will search forward to find
one.  Thus if the cursor is at the start of the line of the previous example,
"%" will search forward and find the first "(".  Then it moves to its match:
```
	if (a == (b * c) / d) ~
	---+---------------->
			   %
```

