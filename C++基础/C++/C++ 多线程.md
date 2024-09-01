1.基础线程类。thread可以用函数对象、lambda、成员函数构造。通常使用的比较复杂、功能性比较强的函数或是用到模板泛型的用function，lambda通常用于局部小功能、简单的判断，可以捕捉局部变量。成员函数通常和私有的成员变量有关系。还有async，异步启动的任务，可以后跟参数决定是否即时启动。packaged_task  是综合了promise、future的封装。
2.变量类。主要是线程返回的异步结果，主要有future（rvalue，只读）、promise（可写）、shared_future（可以当做lvalue，可多线程共享）。
3.锁类。

    recursive_mutex：
        允许同一线程多次锁定同一个互斥锁，而不会导致死锁1。
        适用于递归函数或需要多次进入临界区的场景。
应该是内部添加了原子记数，有相应的线程内部处理逻辑避免不同线程进行锁定。

    shared_mutex（C++17）：
        允许多个线程同时进行读操作，但只有一个线程可以进行写操作。
        适用于读多写少的场景，提高并发性能。
主要用于读写锁，不用手写了直接用了去操作。

    scoped_lock（C++17）：
        同时锁定多个互斥锁，避免死锁。
        自动管理锁的生命周期，简化代码。
简化代码，将多个锁都丢进去，避免上下锁顺序不同而导致的死锁问题。

    std::timed_mutex：支持超时的互斥锁，可以使用try_lock_for和try_lock_until进行带超时的锁操作。
    std::shared_timed_mutex（C++14）：结合了shared_mutex和timed_mutex的功能，支持超时的读写锁。

4.条件类。主要是cv，wait后跟条件谓词，wait_for/wait_until 第二个参数可以指定检查的时间间隔/最长的时间点。然后可以是notify_one/all来告诉随机/所有堵塞等待的线程开始继续执行。当然还有call_once，主要是配合once_flag来保证多线程时某事件只执行一次。

    std::condition_variable：只能与std::unique_lock<std::mutex>一起使用。
    std::condition_variable_any：可以与任何锁类型一起使用。
    notify_all_at_thread_exit：在线程退出时通知所有等待的线程。

4.封装类。如上提到的packaged_task  async（线程类），还有lock_guard    unique_lock adopt_lock、timed_mutex（try_lock_until带时间点)（锁类）、future  promise（变量类）等封装功能的类，主要是利用了RAII的特性。还有C++20的jthread，析构时自动join。
5.thread  detach后就完全失控了。可以将thread放进容器中。
6.信号量。（C++20）

    counting_semaphore：允许多个线程访问共享资源。
    binary_semaphore：类似于互斥锁，但可以手动释放。
内部应该是类似原子的计数器或者利用检查后交换等原子操作的机制。

7.原子操作。

    atomic、atomic_ref、atomic_flag：提供原子操作，避免数据竞争。
    atomic_wait、atomic_notify_one、atomic_notify_all（C++20）：用于原子变量的等待和通知。
