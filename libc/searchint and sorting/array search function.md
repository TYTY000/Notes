#lfind <search.h>
Function: _void *_ **lfind** _(const void *key, const void *base, size_t *nmemb, size_t size, comparison_fn_t compar)_
用于经常增删的数组，搜前排序没用。
不存在返回null。

#lsearch <search.h>
Function: _void *_ **lsearch** _(const void *key, void *base, size_t *nmemb, size_t size, comparison_fn_t compar)_
不存在就追写1条key，并自加nmemb。

#bsearch <stdlib.h>
Function: _void *_ **bsearch** _(const void *key, const void *array, size_t count, size_t size, comparison_fn_t compare)_
binary searches the ***sorted*** array。
