auto f = [] (int a) -> int {
	return a+1;
};
cout <<f(1)<<endl;          //2

特点：
有点像函数，也有类型、参数列表和函数体。但是没有函数名（它其实是一个匿名函数）
**和函数最大的不同在于lambda表达式可以在函数内部定义，现用现写**

一般形式：
[捕获列表] （参数列表） ->返回类型   { 函数体 ;};
需要注意：

*返回类型必须后置，但是可以省略*
如：
auto f = [] (int a) { ... };
但是如果编译器无法推断出返回值类型，就需要显式提供。

*lambda表达式的参数是可以有默认值的*。
auto f = [] (int a = 8) { ... };

*在无参的时候，参数列表可以省略*
auto f1 = [] (){return 1;};
auto f2 = [] {return 2;};

*[ ] 和{ ... }不能省略*
*lambda调用方法也是函数调用运算符*
*可以不返回任何类型（void）*
**不能省末尾的分号**

#捕获列表
1.【】不捕获任何变量（lambda可以直接使用局部静态变量）
2.【&】捕获外部作用域中的所有变量，引用到函数体中。
	int i = 9;
	auto f1 = [&] {
		i = 5; return i;
	};  
	cout<<f1( )<< i <<endl;     //i=5
此时要注意：要确保表达式中引用的变量没有超过变量的作用域（要有有效性），如果超出作用域就不存在了
3.【=】捕获外部作用域中的所有变量，传递（对象）到函数体中。
相当于const，能用，但是不能修改。
	int i = 9;
	auto f1 = [=] {
		i = 5;    //非法
		return i;
	};  
	cout<<f1( )<< i <<endl;     //i=9
4.【this】捕获指针，用于内部，让表达式拥有类内部的权限，可以访问成员变量。
*如果类内已经使用了&/=，就默认添加了此项*
和【=】一样，可以读取但不能修改。想用修改，必须用【&】
5.按值捕获和按引用捕获
【变量名】只按值捕获该变量，不可修改
【&变量名】按引用捕获该变量，可以修改
6.【=，&变量名】 => 【=】+【&变量名】
7.【&，变量名】=》 【&】+【变量名】

	int x = 5;
	auto f = [&] {
		return x;
	};
	 x = 10;
	cout << f() << endl;      //10，&是最终的值，即时访问外部变量
	int y = 5;
	auto f = [=] {
		return y;
	};
	 y = 10;
	cout << f() << endl;      //5,=是当时的值，临时对象拷贝
class CT{
public:
	int i=5;
	void myfunc(int x ,int y ){
		auto mylambda = [this]{return i;	};
		cout<<mylambda()<<endl;
	}
};
	CT ct;
	ct.myfunc(3,4); //5

#lambda_mutable
不管是不是const的变量，只要有mutable，就能修改：
	int x = 5;
	auto f = [=] ( )mutable{     //必须有圆括号
		x = 6;    //虽然是按值捕获不让修改，但是只要有mutable就能改
		return x; 
	};
	 x = 10;
	cout << f( ) << endl;

#lambda表达式的类型和存储
lambda表达式的类型成为Closure Type
可以理解为函数内定义的函数，但是不用传递参数
lambda会为每个表达式单独生成lambda子类（有点像hash），很方便就地定义匿名函数。当用auto定义一个lambda表达式时，就相当于定义了一个匿名的类的对象，
类似一个临时函数，也拥有自身的作用域，是一种比较特殊的匿名的类的对象
	function<int(int)> fc1 = [ ] ( int i ) {return i ;};
	cout<<fc1(15)<endl;       //15
	function<int(int)> fc2 = bind( [ ] (int i){return i;}, 16);
	  //bind  第一个参数是函数指针，第二个是函数参数
	cout << fc2(15)<<endl;     //16     
	//这个lambda表达式其实就是一个匿名函数.

lambda表达式也可以当做普通函数指针
using functype = int (* )(int );
functype fp = [ ] (int i) {return i;};
cout << fp(17) <<endl;          //17
这里要注意，普通函数是没有成员变量的，全靠参数输入，因此就没有捕获列表，【】为空

for_each中的lambda表达式
	vector<int> myvector = { 10,20,30,40,50 };
	int isum = 0;
	for_each(myvector.begin(), myvector.end(), [&isum](int val) {
		isum += val; cout << val << endl;
		});          //isum = 150;

find_if中的lambda表达式
	find_if(myvector.begin(), myvector.end(), [](int val) {
		if (val > 15) return true; return false;
		});
**find_if遇到true就停止遍历，false就继续遍历。**
	if(result == myvector.end()){
		cout<<"没找到"<<endl;
	}else{
		cout<<"找到了"<<endl;
	}

==总结：善用lambda，代码更简洁，更灵活，更强大，开发效率和维护性更高==

vector<function<bool(int)>> gv;
void myfunc() {
	srand((unsigned)time(NULL));
	int tmpvalue = rand() % 6;
	gv.push_back(
		[&](int tv) {        //应该用=
			if (tv % tmpvalue == 0)
				return true;
			return false;
		}
	);
}
	myfunc();
	cout << gv[0](10) << endl;
编译通过，但运行会产生未定义行为。因为&调用的是实际状态，此时&的非静态tmpvalue已经被销毁了。换成=拷贝一个tmpvalue的临时对象就可以了

如果调用的是一个指针，那就还要注意指针的对象的内存/实际状态，不要指向无效的内存。

lambda表达式  【=】、【this】依赖于对象的存在，如果对象被删除，表达式就不再安全了。为了安全，成员函数直接使用变量副本，就能够正确执行表达式了。

#广义lambda捕获
直接把想用的成员变量赋值语句放在捕获列表中
[tmpvalue = m_i].....      //表达式中直接正常使用即可

#lambda的静态局部变量
   ==lambda不需要捕获静态变量，因为可以直接引用，但是不同位置的不同lambda表达式用的static状态不同，结果也不一样，很容易用错。==

