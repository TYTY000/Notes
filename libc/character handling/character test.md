These functions are declared in the header file ctype.h.

Function: _int_ **islower** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-islower)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a lower-case letter. The letter need not be from the Latin alphabet, any alphabet representable is valid.

Function: _int_ **isupper** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isupper)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is an upper-case letter. The letter need not be from the Latin alphabet, any alphabet representable is valid.

Function: _int_ **isalpha** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isalpha)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is an alphabetic character (a letter). If `islower` or `isupper` is true of a character, then `isalpha` is also true.

In some locales, there may be additional characters for which `isalpha` is true—letters which are neither upper case nor lower case. But in the standard `"C"` locale, there are no such additional characters.

Function: _int_ **isdigit** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isdigit)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a decimal digit (‘0’ through ‘9’).

Function: _int_ **isalnum** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isalnum)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is an alphanumeric character (a letter or number); in other words, if either `isalpha` or `isdigit` is true of a character, then `isalnum` is also true.

Function: _int_ **isxdigit** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isxdigit)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a hexadecimal digit. Hexadecimal digits include the normal decimal digits ‘0’ through ‘9’ and the letters ‘A’ through ‘F’ and ‘a’ through ‘f’.

Function: _int_ **ispunct** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-ispunct)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a punctuation character. This means any printing character that is not alphanumeric or a space character.

Function: _int_ **isspace** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isspace)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a _whitespace_ character. In the standard `"C"` locale, `isspace` returns true for only the standard whitespace characters:

`' '`

space

`'\f'`

formfeed

`'\n'`

newline

`'\r'`

carriage return

`'\t'`

horizontal tab

`'\v'`

vertical tab

Function: _int_ **isblank** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isblank)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a blank character; that is, a space or a tab. This function was originally a GNU extension, but was added in ISO C99.

Function: _int_ **isgraph** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isgraph)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a graphic character; that is, a character that has a glyph associated with it. The whitespace characters are not considered graphic.

Function: _int_ **isprint** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isprint)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a printing character. Printing characters include all the graphic characters, plus the space (‘ ’) character.

Function: _int_ **iscntrl** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-iscntrl)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a control character (that is, a character that is not a printing character).

Function: _int_ **isascii** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Classification-of-Characters.html#index-isascii)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

Returns true if c is a 7-bit `unsigned char` value that fits into the US/UK ASCII character set. This function is a BSD extension and is also an SVID extension.