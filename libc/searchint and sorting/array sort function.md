#qsort <stdlib.h>
Function: _void_ **qsort** _(void *array, size_t count, size_t size, comparison_fn_t compare)_
不稳定排序。

#hsearch <search.h>

Function: _int_ **hcreate** _(size_t nel)_
一个程序里通常只能有1个哈希表，当容量达到8成时效率很低；冲突比较少的时候能够实现较好的处理时间。
*返回非0就是正常。*

Function: _void_ **hdestroy** _(void)_
需要注意这个函数不会释放表中的内容，这是代码的责任。
**需要通过额外的数据结构去释放空间（因为哈希表没有遍历函数）**
所以这种哈希表一般是用到程序终止的。
struct entry{
	char \*key;
	void \*data;
} ENTRY;

Function: _ENTRY *_ **hsearch** _(ENTRY item, ACTION action)_
action 有 FIND 和 ENTER，都是找不到后干的动作。
如果是FIND，那就返回NULL ptr，如果是ENTER，就是新增词条。

****
这些函数都是基于struct hsearch_data的数据结构来实现的，它的任何内容都不应被更改，并且被视为private的。
****

Function: _int_ **hcreate_r** _(size_t nel, struct hsearch_data *htab)_
这种是用户手动创建的、基于htab对象的哈希表（可以存在多个，使用的内存是动态分配的，调用前需要清零）。
Function: _void_ **hdestroy_r** _(struct hsearch_data *htab_
该函数能够自动释放相关对象。
Function: _int_ **hsearch_r** _(ENTRY item, ACTION action, ENTRY \*\*retval, struct hsearch_data *htab)_

#tsearch
略。

