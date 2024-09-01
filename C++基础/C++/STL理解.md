
1.容器类。
	map、unordered_map  hash实现的pair{k,v}容器可以用方框下标，实际上使用key值做下标，底层是hash桶的实现，每组数据为pair。
	常用的find/count/insert/erase方法。
	vector定长数组。gcc2倍深拷贝扩容，数组下标访问。vector<bool>除外，这实际上是个位图。常用的有emplace_back, push_back（还有front）, insert, front, back, clear等等
	array不会类型衰退的模板定长数组。
	set、unordered_set hash实现的单key容器。count/insert/erase/clear等。
	list顺序遍历的乱序存储的容器。主要是有一个列表切分的函数。
	树  实际上是多链接链表。分为AVL、BST、RBT等。STL模板抽象了这些关系，将其隐藏于库内部。
	pq本质上是个堆，和map和set等有序（表示先后有逻辑大小关系）容器可以指定排序器。
	跳表实际上就是个四向list，有高度的左右连接的hash桶。
	
2.适配器。
    容器适配器：如stack、queue、priority_queue，这些适配器封装了底层容器，提供特定的接口。
    迭代器适配器：如reverse_iterator、insert_iterator、istream_iterator、ostream_iterator等，用于改变迭代器的行为。
    函数适配器：如bind、function、mem_fn等，用于将函数对象、成员函数或其他可调用对象适配为标准形式。

3.函数类。
	没什么好说的，就是函数模板。lambda匿名函数实现其实是匿名类重载函数运算符。还有成员函数也可以。

4.排序器。
	没什么好说的，有std::less, 等。可以自行定义，用于PQ、MAP（隐式排序容器）、sort（显式排序函数）的参数。

5.迭代器。
	指定了begin和++后就可以用for遍历（无下标）的语法。unordered_map等无序容器内部也可以这么用，这种容器没有排序关系，只有遍历关系（体现为能++ 不能 +2乱序访问、回退等）。c/r/ + begin/end，还有顺序乱序访问，可写不可写。

6.算法：
    STL提供了大量的算法，如排序、搜索、变换、合并等。常用的有sort、find、accumulate、transform、copy等。

7.内存管理：
    STL提供了内存分配器（allocator），用于自定义内存分配策略。默认的分配器是std::allocator。

8.并行算法：

    sequenced_policy：同 C++17 之前，顺序执行。
    parallel_policy：并行执行算法，如果采用多线程方式的话，每一个线程内的执行是顺序的、但是顺序不确定（即上次函数调用结束再进行下一次调用）。
    parallel_unsequenced_policy：多线程 + 向量指令集，线程内、线程间均是无序的。

标准库预定义了三类策略的全局对象：

    std::execution::seq
    std::execution::par
    std::execution::par_unseq
