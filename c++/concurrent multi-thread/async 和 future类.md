#async和future
\# include <future>
std::async        std::future
async是一个启动异步任务的函数模板，会返回一个future对象（类模板）
它会执行线程入口函数，返回一个含有这个函数的返回结果的对象（future.get())
int mythread() {
	cout << "start" << " threadid = "<<this_thread::get_id() << endl;
	//MyCAS* p_a = MyCAS::GetInstance();
	cout << "end " << " threadid = " << this_thread::get_id() << endl;
	return 5; 
}
	cout << "main threadid = " << this_thread::get_id() << endl;
	future <int> result = async(mythread);   //函数的参数从第二个开始放
	cout << "continue ! " << endl;
	cout << result.get() << endl;       **//get到结果，get不到结果就死等**
	cout << "end main" << endl;
	return 0;
result.wait()就是等线程返回，是void的

**引用类中的成员函数（而且还带引用类的成员做参）**
future <int> result = async(&A::mythread, &a,variables); 
//在引用A的成员函数和对象的基础上，再去引用对象的成员。

**async的额外参数**
额外参数专门有个类(枚举类)：

1.std::launch::deferred
如果到wait/get的时候还没调用，就不执行了。
auto result = async( launch::deferred,&A::mythread,&a ,variables)

2.std::launch::async
强制创建新线程去执行
auto result = async( launch::async,&A::mythread,&a ,variables)

3.launch::deferred | launch::async
  或的意思就是
	  要不等到调用result,get的时候再去执行，
	  要不就马上另起线程执行

4.async 和thread的区别

其实async可能不会创建进程，但是能够确保future.get()能拿到

#std::packaged_task
声明方式（两选一）：
**一个包装起来的对象**
	packaged_task<int(int)> mypt(mythread);
或者是：
**lambda表达式**
	packaged_task<int(int)> mypt([](int i)
	{
		。。。。//原函数体内容
	});
.......
	thread t1(ref(mypt), 1);
	//future <int> result = async(launch::deferred|launch::async,mythread,1);
	future <int> result = mypt.get_future();
	//mypt(105);                  //可以直接调用

进阶：任务的向量容器
**直接用容器放包装的任务**
vector<packaged_task<int(int)>> mytasks;

	packaged_task<int(int)> mypt(mythread);
	mytasks.push_back(move(mypt)); //入向量容器
	packaged_task<int(int)> mypt2; //出的任务容器
	auto iter = mytasks.begin();	//获取头指针
	mypt2 = move(*iter);           //取出第一个对象
	mytasks.erase(iter);           //释放第一个元素
	mypt2(123);					   //可以直接调用了

#std::promise
把一个线程的结果保存在进程的全局变量中
void mythread(promise<int> &tmpresult,int calc){
	//....计算过程
	int result = calc;
	tmpresult.set_value(result);             //tmpresult是一个已声明的全局对象
}

	promise<int> mypro;
	thread t1(mythread, ref(mypro), 180);
	t1.join();            //不能少
	future<int> fu = mypro.get_future();
	auto result = fu.get();
	cout << "result = " << result << endl;
还可以再写一个存数字的函数，线程去存  两者结合起来就是线程间的数据传递。
void mythread2(future<int>& tmpf) {
	auto result = tmpf.get();
	cout << "mythread2 result = " << result << endl;
	return;
}

	promise<int> mypro;
	thread t1(mythread, ref(mypro), 180);
	t1.join();
	future<int> fu = mypro.get_future();
	//auto result = fu.get();
	//cout << "result = " << result << endl;
	thread t2(mythread2, ref(fu));   //直接把future对象传进去保存
	t2.join();
这种方法，future只能用一次get，可以用future.vaild() 返回对象是否存在

#future_status
(Enum):timeout,ready,deferred

future_status status = result.wait_for(chrono::seconds(1));
if (status == timeout/  ready  /  deferred){......}

#shared_future
用来解决  多个线程间共享数据的问题
shared_future 类的get  应该是给一份复制，所以就可以多次get
		shared_future <int>result_s(move(fu));         //声明对象
		bool ifcanget = result_s.valid();                      //判断合法性
		auto results = result_s.get();                          //可以多次get