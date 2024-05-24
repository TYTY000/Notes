临界区 (Critical Section)是Win32中提供的一种**轻量级的同步机制**，与互斥 (Mutex)和事件 (Event)等内核同步对象相比，临界区是完全在用户态维护的，所以仅能在同一进程内供线程同步使用，但也因此无需在使用时进行用户态和核心态之间的切换，工作效率大大高于其它同步机制。
**临界区是一个对象，需要被创建和初始化。**
\#include <windows.h>
\#define_WINDOWSLJQ_      //这是一个开关
class A
{
public: 
		A(){
		\#ifdef \_WINDOWSLJQ_           //如果开关打开了
				InitializeCriticalSection(&my_winsec);   //需要构造和析构
		\#endif
		}
		virtual ~A(){
		\#ifdef \_WINDOWSLJQ_
				DeleteCriticalSection(&my_winsec);
		\#endif
		}
}

在成员函数中：
\#ifdef  \_WINDOWSLJQ_
		EnterCriticalSection(&my_winsec);
		//......进行操作
		LeaveCriticalSection(&my_winsec);
\#else                  //相当于
		mymutex.lock();
		//......进行操作
		mymutex.unlock();
\#endif

类成员：
\#ifdef \_WINDOWSLJQ_
CRITICAL_SECTION my_winsec;
\#else
mutex mymutex;
\#endif
};

进出临界区和开关锁是等价的，但是临界区能够多次调用（mutex不能）

再给临界区补充一个包装类：
class CWinLock{
public:
		CWinLock(CRITICAL_SECTION * pCrit){
			m_pC = pCrit;
			EnterCriticalSection(m_pC);
		}
		~CWinLock(){
			LeaveCriticalSection(m_pC);        ** //注意：不用delete * m_pC**
		}
private:
		CRITICAL_SECTION m_pC;
};
在成员函数中：
\#ifdef  \_WINDOWSLJQ_
		CWinLock wlock1(&my_winsec);         //可以多次重复进入临界区
		CWinLock wlock2(&my_winsec);		
		//......进行操作                                      //自动析构，和lock_guard一样
\#else                  //相当于
		lock_guard<mutex> lguard(mymutex);
		//......进行操作
\#endif

