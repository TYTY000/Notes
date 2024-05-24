声明 int * p1 = new int;
初始化int * p2 = new int();
值初始化int * p3=new int(100);


operator new()/delete()
可以重载操作符
但是还有
operator new[]/delete[]
在创建时只会进行1次[]，会进行相应次数的()
然后分配出来的数组头指针其实内容是size值
然后在这个数组前后其实还会占用一定内存去记录debug、字节填充、尾信息等内容

在此引入内存池的概念，类似线程池，一块一块地分配出去，目的是减少内存浪费，但顺带也能够提高程序效率。
**其实就是用了链表结构，用双指针记录当前首地址和空地址，如果满了继续分配新的内存块**


class A {
public:
	static void * operator new(size_t size);
	static void operator delete(void* phead);
	static int count;
	static int a_count;
private:
	A* next;
	static A* freePosi;
	static int trunkCount;             //基本单位
};

void* A::operator new(size_t size) {
	A* tmplink;
	if (freePosi == nullptr) {    //不存在内存指针(第一次用或者是用完了)
		size_t realsize = trunkCount * size;           //算下新的大小
		freePosi = reinterpret_cast<A*> (new char[realsize]);     //开个新的头
		tmplink = freePosi;     // 暂存头指针
		for (; tmplink != &freePosi[trunkCount - 1]; ++tmplink) {
			tmplink->next = tmplink + 1;
			++a_count;
		}  //不断的链接空地址
		tmplink->next = nullptr;//最后一个是空指针
		freePosi = freePosi->next;
		++a_Count;//补齐个数
		return tmplink;
	}
}
void A::operator delete(void* phead) {
	(static_cast<A*>(phead))->next = freePosi;//直接把链表解绑
	freePosi = static_cast<A*>(phead);      //返回本指针
}    //此处其实没有真正释放内存

int A::count = 0;   //记录写入次数
int A::a_count = 0;  //记录内存分配次数
A* A::freePosi = nullptr;    //可以插入的指针
int A::trunkCount = 10;     //内存块内对象的数目



**其实就是用一个链表做结构，加一点内容间的管理机制，保存内容即可**
