用途：
对于给定的变量名或表达式，能告诉程序员相应的类型，返回操作数的数据类型。
*但是不会计算表达式的值，也不会对相关const和引用属性进行修改*
1.后跟变量
	 const int i = 0;
	 const int& iy = i;
	 auto j1 = i;  //j1 = int
	 decltype(i) j2 =15; //const int
	 decltype(i3) j3 = j2;  //const int &

	decltype(CT::i) a;   //int
	CT tmpct;
	decltype(tmpct) tmpct2; //CT
	decltype(tmpct2.i) mv = 5;  //int
	int x = 1, y = 2;
	auto&& z = x; //int &
	decltype(z) && h = y;  //int&

2.后跟表达式  返回结果的类型
	decltype(8) kkk = 5;      //int kkk =5;
	int i = 0;
	int* pi = &i;                   
	int& iy = i;
	decltype(iy + 1)j;         // int j;
	decltype(pi) k;               //int* k;
	* pi = 4;                   //pi = int &
	decltype(i) k2;      //int k2;
	decltype(* pi) k3 = i;         //int * k3 = i ;k3 = int&


如果后接一个非变量的表达式，且该表达式能够作为左值，那么返回结果就是一个引用类型，**decltype((  variable ))的结果永远是引用。**

3.后跟函数
如果是调用函数，就返回函数的返回类型
如果是调用函数名（指针），那就返回函数指针类型
如果是标准库function类型：
	function <decltype(testf)> ftmp = testf;  //testf是int(void)函数指针
ftmp代表一个可调用函数对象（输出一个int）
右侧是一个函数的赋值，个人理解类似管道，相当于输入了一个int
那么  ftmp就是一个 输入int  输出int 的可调用函数对象
	decltype(testf) * tmpf1;       //int (* )(void)


#decltype主要用途
1.应付可变类型
例：常量容器的特点就是不能修改，不能增减删，必须在定义时就初始化，而且迭代器也是常量迭代器类型。
下面是一个定义了迭代器的类C的常量容器对象tmpc中调用迭代器的操作：
iter = tmpc.begin( );     //会编译报错，因为迭代器也是常量迭代器
麻烦的老方法：类模板特偏化，直接把类C中的成员写成const
方便的新方法：把类中的迭代器稍作修改：
	decltype(T( ).begin( )) iter
就会自动读取临时对象的类型，调用临时对象的begin( )，灵活调度
又或者：类A中有一个返回int的成员函数func，
decltype(A( ) , func( )) aaa;        //int aaa;

2.抽取变量类型

3.结合auto 构成返回类型后置语法
函数后置返回类型：
auto func( int a, int b) -> int{ }
此处auto 类型表示延后、后置。
又如：
auto func( int i , int k) -> decltype (i+k)
{
	return i+k;
}

这种写法就很方便，不用折回头来细究、返回类型
但是很明显  不能前置decltype，因为没看见函数模板的参数列表，编译器无法编译，会报错。

4.decltype(auto)      ==>C++14
a.
template <typename T>
auto func(T& t1) {
	t1 *= 2;
	return t1;
}
	int a = 100;
	func(a) = 20;          //报错，表达式必须是可修改的左值
	cout << a << endl;
稍作修改
template <typename T>
decltype(auto) func(T& t1) {
	t1 *= 2;
	return t1;
}          //搞定。

此处是因为auto会把引用类型抛弃（同时会把对象的const也抛弃）导致func返回的是int，不是int& ，就报错了。 

decltype(func(a)) b = a;       //b= int&

b.同样，也可用于变量声明
x=int   ,   y= const int &;
auto z = y; //z=int
decltype(auto) z2 = y;       //z2 = const int &

c.         (x)
int i =10;
decltype (( i )) iy3 = i;  //iy3 = int&;
decltype(auto) func2(){
int i  = 1;
return ( i );
}** //函数返回类型也是int &，但是不能使用返回的这个东西（不是变量，应该是某个地址），也就代表着decltype的特质：不调/使用相关内容**


