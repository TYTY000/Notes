
#explicit_bzero
Function: _void_ **explicit_bzero** _(void *block, size_t len)_
强制写0 ， 覆盖原内容，不会被编译器优化掉。
其他系统可能名字不一样，such as `explicit_memset`, `memset_s`, or `SecureZeroMemory`.

#strfry    <string.h>
Function: _char *_ **strfry** _(char *string)_
进行一个就地字符串重组，这个调用不会影响全局的随机数生成器。

#memfrob   <string.h>
Function: _void *_ **memfrob** _(void *mem, size_t length)_
可以恢复的就地打乱，不需要提供秘钥。类似rot13，用两次就恢复成原样。
专门的加密库是libgcrypt。

#a64l/l64a
Function: _long int_ **a64l** _(const char *string)_
Function: _char *_ **l64a** _(long int n)_
在long 和 小端的ascii码之间互相转换。
![[Pasted image 20230208213511.png]]
