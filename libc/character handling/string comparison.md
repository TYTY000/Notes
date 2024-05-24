
#memcmp
Function: _int_ **memcmp** _(const void *a1, const void *a2, size_t size)_
**需要注意，联合体中不同的类型、结构对象中强制对齐、预分配空间内\\0后面的内存内容等，都会制造出“洞”，导致比较上出现UB。**

#strcmp
Function: _int_ **strcmp** _(const char *s1, const char *s2)_
strcmp不考虑排序规定，要用strcoll

#strcasecmp
Function: _int_ **strcasecmp** _(const char *s1, const char *s2)_
ignore case，但是需要在具体的locale中。

#strncmp
Function: _int_ **strncmp** _(const char *s1, const char *s2, size_t size)_
限制长度的strcmp

#strncasecmp
Function: _int_ **strncasecmp** _(const char *s1, const char *s2, size_t n)_
(case + n) cmp

#strverscmp
Function: _int_ **strverscmp** _(const char *s1, const char *s2)_

目的：用于ls -v    version sort/compare
如果两个字符串中没有数字，和strcmp一样；
相同：返回0。
不同：找到第1个不同的字符，识别出此处最长的数字串，如果有1个为空，就和strcmp一样；否则比较这两个数字串的大小，如果是0开头，就在前边加小数点来比较（0越多，越小越靠前）。