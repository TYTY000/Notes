typedef unsigned int uint_t;  //起别名
typedef std::map<std::string , int > map_s_i;
uint_t = 100;
map_s_i mymap;       mymap.insert({first,1});
typedef只能给固定类型起别名，如果想给map <string,T>就不可以
头文件：
	template <typename T>
	using str_map_t = map<string, T>;
cpp:
    str_map_t<int> map1;
    map1.insert({ "first",1 });
其实using和typedef功能是一样的， 不过写出来要比type的看起来明确很多，用using
#using
1.命名空间  
using namespace std;
2.引用基类成员
using Father::func();
3.别名指定
	template <typename T>
	using str_map_t = map<string, T>;

函数指针
using FunType = int (*)(int,int);
一目了然是一个函数指针