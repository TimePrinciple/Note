**JavaScript Object Notation** is a lightweight **data-interchange** format. It is easy for human to read and write. It is easy for machines to parse and generate.

An **object** is an unordered set of name/value pairs. An object begins with $\{_{\space left\space brace}$ and ends with $\}_{\space right\space brace}$. Each name is followed by $:_{\space colon}$, and the name/value pairs are separated by $,_{\space comma}$.

An **array** is an ordered collection of values. An array begins with $[_{\space left\space bracket}$ and ends with $]_{\space right\space bracket}$. Values are sepatated by $,_{\space comma}$.

A **value** can be a string in double quotes, a number, true, false, null, an object, an array. These structures can be nested.

A **string** is a sequence of zero or more Unicode characters, wrapped in double quotes, using backslash escapes. A character is represented as a single character string. A string is very like a C string.

A **number** is very much like a C number, except that the `octal and hexadecimal formats are not used`.

Typically, crate **serde_json** can be used in Rust to parse the JSON files.
