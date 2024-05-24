#memchr
Function: _void *_ **memchr** _(const void *block, int c, size_t size)_
scan memory for char,
返回块中出现的第一个c字节的指针。

#memrchr
Function: _void *_ **memchr** _(const void *block, int c, size_t size)_
从后往前找。

#rawmemchr
Function: _void *_ **rawmemchr** _(const void *block, int c)_
不需要size参数，会往后一直找，如果结果在块外，返回的指针就不确定。
这个函数在找字符串结尾时特别有用。
	rawmemchr (str, '\\0')

#strchr
Function: _char *_ **strchr** _(const char *string, int c)_
strchr ("hello, world", 'l')
    ⇒ "llo, world"
strchr ("hello, world", '?')
    ⇒ NULL
strchr ("hello,world",'0')
	⇒ '\\0'
当函数返回null指针时，无法获取到字符串末尾的信息，此时可以用strchrnul。

#strchrnul
Function: _char *_ **strchrnul** _(const char *string, int c)_
和strchr不同的是，如果没有找到c字符，会返回字符串结尾null字节的位置。

#strrchr

#strstr
Function: _char *_ **strstr** _(const char *haystack, const char *needle)_
可以找子字符串。如果子字符串为空，返回原字符串。

#strcasestr
忽略大小写。

#strspn  string span
Function: _size_t_ **strspn** _(const char *string, const char *skipset)_

计算string 中 第1个不是skipset中的字符之前的子字符串的长度。

#strcspn string complement span
Function: _size_t_ **strcspn** _(const char *string, const char *stopset)_
是strspn的反义词，
计算string 中 第1个是skipset中的字符之前的子字符串的长度。

#strpbrk string pointer break
Function: _char *_ **strpbrk** _(const char *string, const char *stopset)_
和strcspn相似，但是会返回第1个stopset中的字符的指针。