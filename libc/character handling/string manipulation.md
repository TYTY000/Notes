str系的函数处理不定长度  \#include <string.h>
mem系列的函数处理定长内容  \#include <string.h>
wcs系的函数代表wide character string   \# include <wchar.h>

#strlen
char string[32] = "hello, world";
sizeof (string)
    ⇒ 32
strlen (string)
    ⇒ 12

#strnlen \==(strlen (s) < maxlen ? strlen (s) : maxlen)`
char string[32] = "hello, world";
strnlen (string, 32)
    ⇒ 12
strnlen (string, 5)
    ⇒ 5


#memcpy
Function: _void *_ **memcpy** _(void *restrict to, const void *restrict from, size_t size)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-memcpy)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

The `memcpy` function copies size bytes from the object beginning at from into the object beginning at to. ==The behavior of this function is undefined if the two arrays to and from overlap; use `memmove` instead if overlapping is possible.==

The value returned by `memcpy` is the value of to.

Here is an example of how you might use `memcpy` to copy the contents of an array:

struct foo *oldarray, *newarray;
int arraysize;
…
memcpy (new, old, arraysize * sizeof (struct foo));

#strdup
Function: _char *_ **strdup** _(const char *s)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-strdup)

Preliminary: | MT-Safe | AS-Unsafe heap | AC-Unsafe mem | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This function copies the string s into a newly allocated string. The string is allocated using `malloc`; see [Unconstrained Allocation](https://www.gnu.org/software/libc/manual/html_node/Unconstrained-Allocation.html). If `malloc` cannot allocate space for the new string, `strdup` returns a null pointer. Otherwise it returns a pointer to the new string.

#memmove
Function: _void *_ **memmove** _(void *to, const void *from, size_t size)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-memmove)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

`memmove` copies the size bytes at from into the size bytes at to, even if those two blocks of space overlap. In the case of overlap, `memmove` is careful to copy the original values of the bytes in the block at from, including those bytes which also belong to the block at to.

The value returned by `memmove` is the value of to.

#memccpy
Function: _void *_ **memccpy** _(void *restrict to, const void *restrict from, int c, size_t size)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-memccpy)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This function copies no more than size bytes from from to to, stopping if a byte matching c is found. The return value is a pointer into to one byte past where c was copied, or a null pointer if no byte matching c appeared in the first size bytes of from.

#mempcpy
Function: _void *_ **mempcpy** _(void *restrict to, const void *restrict from, size_t size)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-mempcpy)
返回最后写的字节(null)
Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

The `mempcpy` function is nearly identical to the `memcpy` function. It copies size bytes from the object beginning at `from` into the object pointed to by to. But instead of returning the value of to it returns a pointer to the byte following the last written byte in the object beginning at to. I.e., the value is `((void *) ((char *) to + size))`.

This function is useful in situations where a number of objects shall be copied to consecutive memory positions.

void *
combine (void *o1, size_t s1, void *o2, size_t s2)
{
  void *result = malloc (s1 + s2);
  if (result != NULL)
    mempcpy (mempcpy (result, o1, s1), o2, s2);
  return result;
}

This function is a GNU extension.

#memset
Function: _void *_ **memset** _(void *block, int c, size_t size)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-memset)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This function copies the value of c (converted to an `unsigned char`) into each of the first size bytes of the object beginning at block. It returns the value of block.

#strcpy
Function: _char *_ **strcpy** _(char *restrict to, const char *restrict from)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-strcpy)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This copies bytes from the string from (up to and including the terminating null byte) into the string to. Like `memcpy`, this function has undefined results if the strings overlap. The return value is the value of to.

#stpcpy
Function: _char *_ **stpcpy** _(char *restrict to, const char *restrict from)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-stpcpy)
返回copy末尾的位置(null)，用于后续操作
Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This function is like `strcpy`, except that it returns a pointer to the end of the string to (that is, the address of the terminating null byte `to + strlen (from)`) rather than the beginning.

For example, this program uses `stpcpy` to concatenate ‘foo’ and ‘bar’ to produce ‘foobar’, which it then prints.

\#include <string.h>
\#include <stdio.h>

int
main (void)
{
  char buffer[10];
  char *to = buffer;
  to = stpcpy (to, "foo");
  to = stpcpy (to, "bar");
  puts (buffer);
  return 0;
}

#strdupa
Macro: _char *_ **strdupa** _(const char *s)_[](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html#index-strdupa)

Preliminary: | MT-Safe | AS-Safe | AC-Safe | See [POSIX Safety Concepts](https://www.gnu.org/software/libc/manual/html_node/POSIX-Safety-Concepts.html).

This macro is similar to `strdup` but allocates the new string using `alloca` instead of `malloc` (see [Automatic Storage with Variable Size](https://www.gnu.org/software/libc/manual/html_node/Variable-Size-Automatic.html)). This means of course the returned string has the same limitations as any block of memory allocated using `alloca`.

For obvious reasons `strdupa` is implemented only as a macro; you cannot get the address of this function. Despite this limitation it is a useful function. The following code shows a situation where using `malloc` would be a lot more expensive.

#strcat
使用strcat或wcscat函数的程序员很容易被认为是懒惰和鲁莽的。
这个函数逻辑是从头开始找/0，如果拼接多个字符串就非常低效。

10个100字节的字符串  拼接需要扫描5500次。
应该用reallocarray()，做好循环中的容量判断、memcpy、原字符串空间的释放。