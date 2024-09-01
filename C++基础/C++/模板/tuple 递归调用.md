\# include <tuple>
template <int count, int maxcnt, typename... T>  //这是最基础的模板类
class c2 {
public:
	static void mysfunc(const tuple<T...>& t) {
		cout << "value = " << get<count>(t) << endl;
		c2<count + 1, maxcnt, T...>::mysfunc(t);
	}
};

template <int maxcnt, typename... T>     //这是结束调用的地方
class c2<maxcnt,maxcnt,T...> {         //通过调用函数同类型来做判断条件
public:

	static void mysfunc(const tuple<T...>& t) {

	}
};


template <typename... T>
void myfunctuple(const tuple<T...>& t) {   //这是main调用的函数
	c2<0, sizeof...(T), T...>::mysfunc(t);
}


Main：
	tuple<float, int, int> mytuple(12.5f, 100, 20);
	myfunctuple(mytuple);
