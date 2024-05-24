**Compatibility Note:** In pre-ISO C dialects, instead of returning the argument unchanged, these functions may fail when the argument is not suitable for the conversion. Thus for portability, you may need to write `islower(c) ? toupper(c) : c` rather than just `toupper(c)`.

These functions are declared in the header file ctype.h.

Function: _int_ **tolower** _(int c)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | 

If c is an upper-case letter, `tolower` returns the corresponding lower-case letter. If c is not an upper-case letter, c is returned unchanged.

Function: _int_ **toupper** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Case-Conversion.html#index-toupper)

Preliminary: | MT-Safe | AS-Safe | AC-Safe |

If c is a lower-case letter, `toupper` returns the corresponding upper-case letter. Otherwise c is returned unchanged.

Function: _int_ **toascii** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Case-Conversion.html#index-toascii)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | 

This function converts c to a 7-bit `unsigned char` value that fits into the US/UK ASCII character set, by clearing the high-order bits. This function is a BSD extension and is also an SVID extension.

Function: _int_ **_tolower** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Case-Conversion.html#index-_005ftolower)

Preliminary: | MT-Safe | AS-Safe | AC-Safe |



Function: _int_ **_toupper** _(int c)_[](https://www.gnu.org/software/libc/manual/html_node/Case-Conversion.html#index-_005ftoupper)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | 

This is identical to `toupper`, and is provided for compatibility with the SVID.