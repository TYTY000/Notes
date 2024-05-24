1.独占，置空时销毁

2.make_unique(C++14)
也不支持指定删除器
unique_ptr<int> p1 = std:: make_unique<int>(100);
auto p2 = std:: make_unique<int>(200);
unique_ptr <int> p3 (new int(100));

3.不支持复制和赋值指针的动作，只能移动
unique_ptr<int> p4 = std::move(p3);

4.release   
unique_ptr<int>p5(p4.release());
返回裸指针，取消指向绑定
**直接单用会导致内存泄漏**，如果不想用了也要用裸指针接住delete掉：
string * tempp = ps2.release();
delete tempp;

5.reset
unique_ptr<int>p5.reset(p4);  //更新指针，释放原内存
unique_ptr<int>p5.reset(new int (100));  //更新指针，释放原内存
不带参数时就置空

6.=nullptr
释放并置空

7.指向数组
std ::unique_ptr < int[ ]> ptrarray( new int[10]);    //可直接引用

8.get 裸指针

9.swap 交换指针指向

10.判断条件  同强指针

11.转换成强指针
用一个函数返回原值（临时对象可以做右值）
template<typename T>
unique_ptr<T> func(unique_ptr<T>& tmpp) {
	return tmpp;
 }

	unique_ptr<string> up(new string("123123"));
	shared_ptr<string> sp1 = func(up);
	//shared_ptr<string> sp2 = std::move(up);

12.返回unique_ptr
函数返回值是一个临时对象，返回以后接住就可以了
	unique_ptr<string> up1 = func(up);

13.删除器
void mydeleter(string* pdel) {
	delete pdel;
	pdel = nullptr;
}
	using fp = void (*) (string*);   //普通函数指针
	unique_ptr<string, fp> usp(new string("123123"),mydeleter);

	
	unique_ptr<string, decltype(mydeleter)*> usp3(new string("12345"), mydeleter); //返回函数指针

	auto mydellam = [](string* pdel) {
		delete pdel;
		pdel = nullptr;
	};            //lambda 表达式
	unique_ptr<string, decltype(mydellam)> usp4(new string("12345"), mydellam);

==但到了 unique _ ptr 这里，如果删除器不同，则就等于整个  
unique_ptr 类型不同，那么，这种类型不同的 unlqu e_ ptr 智能指钊没有办法放到同一个容器里去的 .==

14.尺寸
大小和裸指针一样大，加了函数删除器要大些，加了lambda删除器 还是一样大