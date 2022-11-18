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
           ge      b          w                             e
           <-     <-         --->                          --->
    This is-a line, with special/separated/words (and some more).
       <----- <-----         -------------------->         ----->
         gE      B                   W                       E
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
            0                  $
```
(the "....." indicates blanks here)

### Move to a character

- `f<char>` command ("f" stands for "Find") searches to the right in the line for the single character **\<char\>**, the cursor will be positioned over that character:
```
    To err is human. To really foul up you need a computer.
    ---------->-------------->
        fh       fy
              -------------------->
                       3fl
```
- `F<char>` command searches to the left:
```
    To err is human. To really foul up you need a computer.
              <---------------------
                        Fh
```
- `t<char>` command ("t" stands for "To") works like the "f" command, except it stops one character before the searched character. And `T` command moves to the left.
```
    To err is human. To really foul up you need a computer.
               <------------  ------------>
                    Th             tn
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
    if (a == (b * c) / d)
    ---+---------------->
               %
```
### Move to a specific line

`G` command: 

- With a count, this command positions the cursor at the given line number.

- With no argument, "G" positions the cursor at the end of the file.  A quick way to

`gg` command: go to the start of a file.
```
        |   first line of a file   ^
        |   text text text text    |
        |   text text text text    |  gg
    7G  |   text text text text    |
        |   text text text text
        |   text text text text
        V   text text text text    |
            text text text text    |  G
            text text text text    |
            last line of a file    V
```

### Jump by percentage

`<num>%` command positions the cursor to \<num\> percent of file.

### Move to the top, middle and bottom of view

- `H` (high) command move to the top of the view.

- `M` command move to the middle of the view.

- `L` (low) command move to the bottom of the view.
```
                +---------------------------+
        H -->   | text sample text          |
                | sample text               |
                | text sample text          |
                | sample text               |
        M -->   | text sample text          |
                | sample text               |
                | text sample text          |
                | sample text               |
        L -->   | text sample text          |
                +---------------------------+
```
### Telling where you are

- `CTRL-G` command gives a message telling where you are, it shows the the name of the file been editing, the line number where the cursor is, the total number of lines, the percentage of the way through the file and the column of the cursor.
```
    "usr_03.txt" line 233 of 650 --35%-- col 45-52 ~
```
- `number` option: Set this will display a line number in front of every line:
```vim
    :set number
    :set nonumber "switch the number option off
```
- `ruler` option: Set thhis will display the cursor position in the lower right corner of the Vim window:
```vim
    :set ruler
```

### Scroll around

- Half a screen:

    - `CTRL-U`: ("U" stands for "up") move up half a screen.

    - `CTRL-D`: ("D" stands for "down") move down half a screen.

- A screen:

    - `CTRL-B`: ("B" stands for "back") move up a screen.

    - `CTRL-F`: ("F" stands for "forward") move down a screen.

- A line:

    - `CTRL-Y`: move up a line.

    - `CTRL-E`: move down a line.

`z[tzb]` command puts the cursor line at the top, middle, bottom.

### Search

`/<content>` command searches for a string include \<content\>, `n` command moves forward to next match, `N` command moves to previous match.

`?<content>` command searches backward for a string containing \<content\>, `n` goes backward and `N` goes forward.

##### Ignore case

If the case is not concerned, cases can be ignored by setting the 'ignorecase' option:
```vim
    :set ignorecase
    :set noignorecase   " Switch the ignorecase option off
```
##### Search for exact match

`*` command searches the exact match of the word under the cursor forward.

`#` searches in the opposite direction.

##### Partial search

`/<content>\>` find words that end in "content".

`/\<<content>` find words that start with "content".

`/\<<content>\>` find the exact match.

##### Enable/Disable hightlight

Vim will highlight all matches. To switch this off:
```vim
    :set nohlsearch
    :set hlsearch   " Turn on the hlsearch option
    :noh            " Abbreviation for 'nohlsearch', remove the highlighting after search
```
##### Search settings

`nowarpscan` option will stop search at the end or start of the file.

`noincsearch` option contorls the display of matches while the typing is still in process.

```vim
    :set nowrapscan     " Switch off the default wrapscan option.
    :set noincsearch    " Disable the display while typing.
```

### Advanced search

- `/<content>` matches all occurences in a line. Mark all `/the` matches with "x"s in a line:
```
    the solder holding one of the chips melted and the
    xxx                       xxx                  xxx
```

- `/^<content>` matches the "content" only if it is at the beginning of a line. E.g. `/^the`:
```
    the solder holding one of the chips melted and the
    xxx
```

- `/<content>$` matches the "content" only if it is at the end of a line. E.g. `/the$`:
```
    the solder holding one of the chips melted and the
                                                   xxx
```

##### Match any single character

The . (dot) character matches any existing character. For example, the pattern "c.m" matches a string whose first character is "c", whose second character is anything, and whose third character is "m". E.g. `/c.m`:
```
    We use a computer that became the cummin winter.
             xxx             xxx      xxx
```
In that case, if the "." is to be searched, escape it with "\", E.g. `/ter\.`:
```
    We use a computer that became the cummin winter.
                  ----                          xxxx
```

### Jump

When a jump to a position with the `G` command, Vim remembers the position from before this jump. This position is called a *mark*.

- ``` `` ``` (or `''`): goes back, This \` is a *backtick* or open *single-quote* character. If use the same command a second time will jump back again. That's because the `` ` `` command is a jump itself, and the position from before this jump is remembered.

Generally, a command that can move the cursor further than within the same line, this is called a *jump*. This includes the search commands "/" and "n". But not the character searches with "fx" and "tx" or the word movements "w" and "e". Also, "j" and "k" are not considered to be a jump, even when a count is used to make them move the cursor quite a long way away.

The ``` `` ``` command jumps back and forth, between two points. The `CTRL-O` command jumps to older positions ("O" stands for older). `CTRL-I` then jumps back to newer positions.
```
         |  example text   ^             |
    33G  |  example text   |  CTRL-O     | CTRL-I
         |  example text   |             |
         V  line 33 text   ^             V
         |  example text   |             |
   /^The |  example text   |  CTRL-O     | CTRL-I
         V  There you are  |             V
            example text
```

`:jumps` command gives a list of positions jumped to. The entry which used last is marked with a ">".

### Marks

Vim enables you to place marks in the text.

- `m<mark>` command marks the place under the cursor as mark **a**. 26 marks (a through z) can be placed in text.

- `` `<mark> `` command goes to the marked position.

- `'<mark>` command goes to the beginning of the line containing the mark.

`:marks` command displays a list of marks.

There are a few special marks:
```
    '   The cursor position before doing a jump
    "   The cursor position when last editing the file
    [   Start of the last change
    ]   End of the last change
```

### `<operator><motion>` command

#### `d` commands:

`dw` command deletes a word. The `d` command may be followed by any `<motion>` command, and it deletes from the current location to the place where the cursor winds up. The `4w` command, for example, moves the cursor over four words. The `d4w` command deletes four words:
```
    To err is human. To really foul up you need a computer.
                     ------------------>
                             d4w
    To err is human. you need a computer.
```
Vim only deletes up to the position where the motion takes the cursor.

`e` command moves to the end of a word, Vim includes that last character:
```
    To err is human. you need a computer.
                    -------->
                       d2e
    To err is human. a computer.
```
Whether the character under the cursor is included depends on the command used to move to that character. The reference manual calls this *exclusive* when the character isn't included and *inclusive* when it is.

`$` command moves to the end of a line. The `d$` command deletes from the cursor to the end of the line. This is an `inclusive` motion, thus the last character of the line is included in the delete operation:
```
    To err is human. a computer.
                   ------------>
                        d$
    To err is human
```
#### `c` commands:

Another operator is "c", change. It acts just like the "d" operator, except it leaves in Insert mode. For example, `cw` changes a word. Or more specifically, it deletes a word and then switch to **Insert mode**.
```
    To err is human
       ------->
         c2wbe<Esc>
    To be human
```
This `c2wbe<Esc>` contains these bits:
```
    c   the change operator
    2w  move two words (they are deleted and Insert mode started)
    be  insert this text
    <Esc>   back to Normal mode
```
But there is something strange: The space before "human" isn't deleted. There is a saying that *for every problem there is an answer that is simple, clear, and wrong*. That is the case with the example used here for the `cw` command. The "c" operator works just like the "d" operator, with one exception: `cw`. It actually works like `ce`, change to end of word. Thus the space after the word isn't included. This is an exception that dates back to the old Vi. Since many people are used to it now, the inconsistency has remained in Vim.

#### Shortcuts:

`dd`: deletes a whole line.

`cc`: changes a whole line. And keeps the existing indnet (leading white space).

`x`: stands for `dl` (delelte character under the cursor).

`X`: stands for `dh` (delete character left of the curosr).

`D`: stands for `d$` (delete to end of the line).

`C`: stands for `c$` (change to end of the line).

`s`: stands for `cl` (change one character).

`S`: stands for `cc` (change a whole line).

#### Combine with `<count>`

The commands `3dw` and `d3w` both delete three words. The first command, `3dw` deletes on word three times; the second command `d3w` deletes three words once. This is a difference without a distinction. E.g. `3d2w` deletes two words, repeated three times, for a total of six words.

#### Replace with one character

The `r` command is not an operator. It waits for a character, and will replace the character under the cursor with it. This can be done with `cl` or with the `s` command, but with `r` you don't have to press <Esc> to get back out of insert mode.
```
    there is somerhing grong here
    rT       rt    rw
    There is something wrong here
```
Using a count with `r` causes that many characters to be replaced with the same character. E.g.:
```
    There is something wrong here
               5rx
    There is something xxxxx here
```
To replace a character with a line break use `r<Enter>`. This deletes one character and inserts a line break. Using a count here only applies to the number of characters deleted: `4r<Enter>` replaces four characters with one line break.

#### Repeat a change

`.` command repeats the last change.

For instance, suppose you are editing an HTML file and want to delete all the "\<B\>" tags. You position the cursor on the first "\<" and delete the "\<B\>" with the command `df>`. Then go to the "<" of the next "</B>" and delete it using the `.` command. The `.` command executes the last **change command** (in this case, `df>`). To delete another tag, position the cursor on the "<" and use the `.` command:
```
                              To <B>generate</B> a table of <B>contents
        f<   find first <     --->
        df>  delete to >         -->
        f<   find next <           --------->
        .    repeat df>                     --->
        f<   find next <                       ------------->
        .    repeat df>                                     -->
```
> The `.` command works for all changes made, except for `u` (undo), `CTRL-R` (redo) and commands that start with a colon ":".

Another example: Change the word "four" to "five". It appears several times in text:
```
        /four<Enter>    find the first string "four"
        cwfive<Esc>     change the word to "five"
        n               find the next "four"
        .               repeat the change to "five"
        n               find the next "four"
        .               repeat the change
```
### Visual mode

To delete simple items the operator-motion changes work quite well. But often it's not so easy to decide which command will move over the text you want to change. Then use Visual mode.

`v` command goes to *Visual mode*. The text been selected is highlighted. Finally type the operator command.

For example, to delete from the middle of a word to the middle of another:
```
        This is an examination sample of visual mode
                       ---------->
                         velllld
        This is an example of visual mode
```

#### Select lines

`V` command goes to *Visual line mode*. In this mode, movement to left or right will be disabled. When you move up or down the selection is extended whole lines at a time.

For example, select three lines with `Vjj`:
```
                       +------------------------+
                       | text more text         |
                    >> | more text more text    | |
     selected lines >> | text text text         | | Vjj
                    >> | text more              | V
                       | more text more         |
                       +------------------------+
```
#### Select blocks

`CTRL-V` command goes to *Visual block* mode. In this mode, the content selected is a rectangular block. Especially useful when working on tables.
```
               name            Q1       Q2      Q3
               pierre          123      455     234
               john            0        90      39
               steve           392      63      334
```
To delete the middle "Q2" column, move the cursor to the "Q" of "Q2". Press `CTRL-V` to start blockwise Visual mode. Now move the cursor three lines down with `3j` and to the next word with `w`.

#### Go to the other side

`o` command selects the cursor on the other side (a selection is done by two cursors, one at the beginning and another at the end).

`O` in *Visual block* mode selects another cursor in the same line.

> Note that `o` and `O` in *Visual* mode work very differently from *Normal* mode, where they open a new line below or above the cursor.

### Move text

The content deleted by command `d`, `x` or other commands is saved. They can be reclaimed by the `p` command. 
### Copy text

`y` command is used to copy text. It is an operator, so the combination of `<operator><motion>` command works well here. It can also be used after selection in *Visual* mode and then press `y`. E.g.:
```
        let sqr = LongVariable *
                 -------------->
                       y2w
        let sqr = LongVariable *
                               p
        let sqr = LongVariable * LongVariable
```
`yy` command yanks a whole line, like `dd` command deletes a whole line.

`Y` command yanks to end of the line.

### Text objects

If the cursor is in the middle of a word and want to delete that word. There is a simpler way to do this: `daw`:
```
    this is some example text.
                   daw
    this is some text.
```
The `d` of `daw` is the *delete operator*. `aw` is a *text object*. (`aw` stands for "A Word", thus `daw` is "Delete A Word"). To be precise, the white space after the word is also deleted (or the white space before the word if at the end of the line).

It is very similar to `<operator><motion>`, but instead of operating on the text between the cursor position before and after a movement command, the text object is used as a whole. It doesn't matter where in the object the cursor was.

To change a whole sentence use `cis`. Take this text:
```
        Hello there. This
        is an example. Just
        some text.
```
Move to the start of the second line, on `is an`. Now use `cis`:
```
        Hello there. Just
        some text.
```
The cursor is in between the blanks in the first line. Now you type the new sentence "Another line.":
```
        Hello there. Another line. Just
        some text.
```
`cis` consists of the "c" (change) operator and the "is" text object. This stands for "Inner Sentence". There is also the "as" ("A Sentence") object.

The difference is that "as" includes the white space after the sentence and "is" doesn't. If you would delete a sentence, you want to delete the white
space at the same time, thus use "das". If you want to type new text the white space can remain, thus you use "cis".

##### Work with visual mode

You can also use text objects in Visual mode. It will include the text object in the Visual selection. Visual mode continues, thus you can do this several times. For example, start Visual mode with "v" and select a sentence with "as". Now you can repeat "as" to include more sentences. Finally you use an operator to do something with the selected sentences.

### Replace mode

`R` command goes to *replace mode*. In this mode, each character typed replaces the one under the cursor:
```
    This is text.
            Rinteresting.<Esc>

    This is interesting.
```

You may have noticed that this command replaced 5 characters in the line with twelve others. The "R" command automatically extends the line if it runs out of characters to replace. It will not continue on the next line.

### 
