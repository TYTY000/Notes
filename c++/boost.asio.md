#endpoint
分tcp和udp，通常用ip+port 来初始化，ip通常是boost::asio::ip::make_address(string)转换的。
也可以用ioc来初始化。
如果是服务器端，ip为boost::asio::ip::address_v4/6::any()

#socket 
也分tcp和udp，用{ioc，ep.protocol()}初始化。
socket.bind(ep)    socket.connect(ep)  
socket.open(protocol)  用于创建一个新的套接字

#acceptor
用于TCP服务器端，绑定的ep是any。
绑定ep		acceptor.bind(ep); 
绑定socket     acceptor.accept(socket)后即可进行接收（堵塞）。
监听新的连接acceptor.listen(backlog)，backlog指定了等待接受的连接队列的最大长度。

#resolver
DNS解析器。
也分tcp和udp。
用ioc初始化：	boost::asio::ip::tcp::resolver resolver(ioc);
然后调用resolver.resolve(host, port)  来返回ep的迭代器。

#buffer
分const和mutable，分别用于输出和输入，对STL自带string_view，string，c_str、array<char,N>、智能指针（如std::make_unique<uint8_t[]>(BUF_SIZE_BYTES)）均有支持，（需要指定长度来修改，string 会转换为c_str）
buffer.size()用于C风格的安全范围计算。
主要是做了溢出保护，兼容boost.asio的其他类。

#stream
兼容STL，可以直接赋值。使用方法同。
还有TCP、UDP特化的iostream，使用方法同。
stream.close()

#error_code
boost.system.error_code用法类似signo。
error_code.message() 用于获取错误的描述信息。

#IO 
需要使用boost.asio.buffer。
不指定长度（读满后抛弃，需要buffer指定长度）：
堵塞读写  write/read(socket, buffer)
socket.send\/receive (socket, buffer)也使用同样的接口。
异步读写  异步的复杂些，需要使用C++11的std::bind或者lambda表达式作为回调函数。

需指定长度（在buffer中指定长度）：
堵塞读写 write/read_some(socket, buffer, ec)
异步读写 async_read/write_some(socket, buffer, ec, handler)  
handler通常是一个函数对象，它接受一个error_code和一个表示传输字节数的参数，如果接口不带ec就会直接throw。
如果操作成功，error_code的值为0，否则为具体的错误码，根据boost::asio::error的枚举类型对应去处理，
如果操作被取消（通常是socket.cancel()，会抛异常），error_code的值将为boost::asio::error::operation_aborted。
以上是tcp，如果是udp请在函数名后加后缀_to，如（send_to)，并将接口中首个参数socket放到最后（UDP本身就是异步的，没有handler）。

**_此外，boost::asio还提供了boost::asio::async_write_at和boost::asio::async_read_at函数，用于异步的随机访问读写操作。_**

#client
通常是将C/S封装进行封装，直接调用对应的方法。
client对象需要目标地址和端口，私有socket、ep、ioc，初始化时用地址、端口、协议进行绑定，其中可以用到：
```cpp
			m_sock.shutdown(
				boost::asio::ip::tcp::socket::shutdown_both   // or send/receive
			);
			m_sock.close();
```
需要进行的动作都在类中进行公开定义、涉及内部数据的操作进行私有定义。
其中也可以附加文件、路径、请求、回复等处理对象的私有定义。
同步tcp需要私有endpoint，socket，ioc，
udp需要endpoint，ioc
他们的逻辑比较简单，送完了读，读完了送，（送、读通常私有，根据需求暴露操作接口）按照上述接口去设置即可。
异步tcp：首先需要定义会话类，需要私有ep, socket, ioc,还要私有收发buffer、错误码、请求序列号、atomic_bool指示；
其次客户端内部私有会话智能指针的容器<\map>、类锁（会话的登记、取消）、线程（初始化自动调用ioc.run()）、work_guard（用来保护ioc多线程？）
  ```cpp
  using work_type =
      boost::asio::executor_work_guard<boost::asio::io_context::executor_type>;
  std::unique_ptr<work_type> m_work{
      std::make_unique<work_type>(boost::asio::make_work_guard(m_ioc))};
// boost::asio::executor_work_guard是一个类，它的作用是阻止io_context::run在没有任何未完成的工作时返回。
// 在你的代码中，work_type是boost::asio::executor_work_guard<boost::asio::io_context::executor_type>的别名，m_work是work_type的unique_ptr。
// 当m_work被销毁（例如在析构函数中），它会自动调用executor_work_guard的析构函数，这将允许io_context::run在所有工作完成后返回。这是一种保护机制，确保io_context::run在你期望的时候返回。
```
此外，客户端还可以使用boost::asio::deadline_timer来设置超时，以防止某些操作无限期地阻塞。

#server
服务器指的都是TCP服务器，UDP是对等通信。
建立类的方法类似，
	单线程：也需要私有ioc、atomic_bool标记、jthread、acceptor和socket（可以不作为私有成员，单独创建一个，做简单封装，在atomic_bool非false时一直监听），定义handler处理client请求。
		**收到请求可以创建一个socket的智能指针然后move给handler，创建一个新线程并且detach（用对象的智能指针保证结束后正常析构，也可以用jthread来减少代码量），也可以创建一个对象进行使用，这个就是并行和串行之分，不是严格意义上的异步。**
	多线程：
		1.智能指针容器管理：每个连接操作都会生成一个智能指针（需要在连接类上enable_shared_from_this并在操作中自增引用计数auto self(shared_from_this())，或者不断的右值move转移拥有者），根据运行逻辑进行链式调用。
		2.ioc池，对每个请求和链接分配一个ioc。（未深入了解，太繁琐？）
		3.线程池，在thread的容器中利用lambda捕捉ioc，在thread的构造中运行ioc.run，需要在server类中加一个信号集并注册信号并进行相应。
		这个策略可以充分利用多核CPU的性能，提高服务器的并发处理能力。但是需要注意的是，线程池的大小不宜设置过大，否则会因为线程切换过于频繁而导致性能下降。一般来说，线程池的大小可以设置为CPU核数的两倍。




#ssl支持
未完待续。。。