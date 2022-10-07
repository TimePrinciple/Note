### -i

Go to interactive mode after the files specified (if exist) has been executed.

### dofile("\<file name>")

This function call loads the specified file (run the file). Can be used for debugging process.

### type(\<variable name>)

Returns a string denotes the type of the variable.

`thread`, `table`, `userdata` are lua specific types.

### `--` and `--[[`, `--]]`

Comments of lua. To disable a long comment, precede a `-` which makes `---[[`.

### `arg`

A script can retrieve its arguments through the predefined global variable `arg`. The **script name** goes into `index 0`; its **first argument** goes to `index 1`, and so on. Preceding options go to negative indices, as they appear before the script.

### \#: lengthoperator

This operatior counts the length in bytes, which is not the same as characters in some encodings

### ..: concatenation ooperator

This operator creates a new string, without any modification to its oprands.

### [[ ]]: long string (raw string)

Escape sequence is not interpreted in the two double square brakets.Any number of equal signs can be added between the two opening brakets and closing brakets.

### tonumber(\<string>)

Converts the string to a number, returns `nil` if failed.

### tostring(\<number>)

Converts the number to a string.

### `string` library

Calls to functions in `string`library are invoked by `string.<function name>()`.

### `utf8` library

A library designed to handle UTF-8 strings in program. Usually used functions are `len()`, `char()` and `codepoint`.

### Tables

Tables are the main (in fact, the only) data structuring mechanism in Lua, they are used to represent arrays, sets records, and many objects as well. A table accepts not only `numbers` as indices, but also `strings` or any other value of the language, except `nil`.

### { }: constructor expression

Creates an empty table. The first element of the constructor has index 1, not 0.

### `.` & `[ ]` access notation

`.` converts the following content to a string and then access the table, while `[ ]` access a table as it is.

###  
