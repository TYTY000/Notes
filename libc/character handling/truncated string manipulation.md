这些函数会突然截断字符串，还容易使多字节字符内部截断失效，一般不用。
截断函数可以掩盖原本会被自动技术捕获的应用程序错误，所以应该仅在应用程序的底层逻辑需要截断时使用这些函数。

#strncpy
Function: _char *_ **strncpy** _(char *restrict to, const char *restrict from, size_t size)_ 

只复制copy size字符，**少的时候后面全补null，如果超出就不会往末尾补null。**

#strndup
Function: _char *_ **strndup** _(const char *s, size_t size)

强复制字符串，最多也只复制size字符 ，**总是会null结尾。**

#strndupa
Macro: _char *_ **strndupa** _(const char *s, size_t size)_

也是堆栈空间复制字符串。

#stpncpy
Function: _char *_ **stpncpy** _(char *restrict to, const char *restrict from, size_t size)_

只复制copy size字符，**少的时候后面全补null，如果超出就不会往末尾补null。**
*返回最后一个null指针，如果没有（复制满了）就返回最后一个复制字符的后一个字符的指针。*
