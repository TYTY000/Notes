程序一般都要进行词义分析和解析。

#strtok
Function: _char *_ **strtok** _(char *restrict newstring, const char *restrict delimiters)_
通过重复的调用，可以把newstring拆分为若干个token。
要分割的字符串只会在第一次调用的时候作为char \*newstring 进行传递，strtok函数会用其进行内部初始化。后续用null进行重复调用可以继续分割其他token。
**没有其他库函数会调用strtok来破坏子字符串。**
delimiter作为用于判定的分界字符串，会被丢弃，可以通过分界符前后来判断token的前后界。

**strtok会改变原字符串，拷一份副本传进去用。**
*如果直接处理const char \* 会报错。*
**如果程序的目的不是修改某个数据结构，那么临时的修改很容易出问题。**

\#include <string.h>
\#include <stddef.h>

…

const char string[] = "words separated by spaces -- and, punctuation!";
const char delimiters[] = " .,;:!-";
char *token, *cp;

…

cp = strdupa (string);                /* Make writable copy.  */
token = strtok (cp, delimiters);      /* token => "words" */
token = strtok (NULL, delimiters);    /* token => "separated" */
token = strtok (NULL, delimiters);    /* token => "by" */
token = strtok (NULL, delimiters);    /* token => "spaces" */
token = strtok (NULL, delimiters);    /* token => "and" */
token = strtok (NULL, delimiters);    /* token => "punctuation" */
token = strtok (NULL, delimiters);    /* token => NULL */

#strtok_r
Function: _char *_ **strtok_r** _(char *newstring, const char *delimiters, char \**save_ptr)_
strtok的可重入版本，把地址存在save_ptr中，后续调用时 newstring 仍然为null。

#strsep
Function: _char *_ **strsep** _(char \**string_ptr, const char \*delimiter)_
类似重入版本，但是会在多个分隔符中间返回空字符串，**所以strsep的结果需要test。**

\#include <string.h>
\#include <stddef.h>

…

const char string[] = "words separated by spaces -- and, punctuation!";
const char delimiters[] = " .,;:!-";
char \*running;
char \*token;

…

running = strdupa (string);
token = strsep (&running, delimiters);    /* token => "words" */
token = strsep (&running, delimiters);    /* token => "separated" */
token = strsep (&running, delimiters);    /* token => "by" */
token = strsep (&running, delimiters);    /* token => "spaces" */
token = strsep (&running, delimiters);    /* token => "" */
token = strsep (&running, delimiters);    /* token => "" */
token = strsep (&running, delimiters);    /* token => "" */
token = strsep (&running, delimiters);    /* token => "and" */
token = strsep (&running, delimiters);    /* token => "" */
token = strsep (&running, delimiters);    /* token => "punctuation" */
token = strsep (&running, delimiters);    /* token => "" */
token = strsep (&running, delimiters);    /* token => NULL */

#basename
Function: _char *_ **basename** _(const char *filename)_

