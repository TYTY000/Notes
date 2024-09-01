#placement-new
能在一个预先分配好的内存地址中构造一个对象。
更灵活
class A{
	int m_a;
	A():m_a(0){  	.....   }
	A(int tmp): m_a(tmp){   ...   }
	~A(){   ...   }
}

void* mempoint = (void * )new char[sizeof(A)];
A* myobj1 = new(mempoint) A();
delete[] (void * )myobj1;