#auto
用于变量的自动类型推断，不需要程序员显式指定类型
*特点
1.auto的自动类型推断发生在编译期间
2.auto定义的变量必须立即初始化，编译器才能进行推断，进而在编译期间进行类型替换
3.auto可结合指针、引用、const等限定符使用*
auto 代表一个类型。下面按照形参类型进行区分：

**1.按值传递**（对象，非指针，非引用）
auto x= 5;       //int
const auto x2 = x;         //const int
const auto& xy = x;
auto xy2= xy;        //xy2=int

*auto会抛弃引用、const等限定符。*

**2.指针或普通引用**
const auto& xy = x;        // xy = const int &, auto = int
auto xy3 = xy;                //xy3  = const int &,auto = const int
auto y = new auto (100);          // y = int * ; auto = int * ;
const auto * xp = &x ;           //xp = const int * ; auto = int
auto * xp2 = &x ;     // auto = int ; xp2 = int * ;
auto xp3 = &x ;     //auto = int *  ; xp3 = int* ;

*auto不会抛弃const，但会抛弃引用。*

**3.万能引用**
auto&& nini1 = 222;          //nini1 = int &&;auto = int
auto&& nini2 = x;              //auto  = int &;   nini2 = int &
auto&& nini3 = x2             //x2:const int &         nini3:const int & auto :const int  &

**4.数组**
const char mystr[] = “123123”;         //const char[7]
auto myarr = mystr;                          //const char * ;
auto& myarr2 = mystr;                     //const char(&) [14]
int a[2] = {1,2}
auto aauto = a;                                //int * 

**5.函数**
void myfunc3(double,int ){  ...  }
auto tmpf = myfunc3;           //void (* )(double,int)
auto& tmpf2 = myfunc3;        //void(&)(double,int)
tmpf(1,2); tmpf2(3,4);

**6.std::initializer_list** 
int x = 10;              //C++98
int x2(20);              //C++98
int x3 = {30};         //C++11
int x4{40};             //C++11

其中注意：
auto x = {30};      //std::initializer_list<int>
std::initializer_list      是C++11引入的一个新类型，表示某种特定类型的数组，类似向量。
注意观察“={}”形式，这就是表示某种特定类型的值的数组。
编译器先判断出initializer_list的类型，再去判断成员类型，回头再加入模板类
==类型要统一，否则也会造成歧义。==

template <typename T>
void fautof(std::initializer_list<T> param) {   ...   }
	fautof({12});

#auto 不适用场合举例
1.不能用于函数参数
2.不能用于非静态成员外的成员定义
class A{
public:
	//auto i = 12;            //普通成员不能用，如果想泛型应该用模板
	static const auto j = 15;       //使用auto的时候值必须在类内初始化。static const就相当于定义。
}

#auto适用场合举例
1.迭代器
std::map <string,int> mymap;
mymap.insert({"aa",1});
mymap.insert({"bb",2});
mymap.insert({"cc",3});
mymap.insert({"dd",4});
for循环里面直接用auto
for(auto iter = mymap.begin()........)    //节省一点代码量，不用声明iter类型
2.函数变量的保存