#Cuda函数限定符
_____global_____
device执行 host和device调用  异步
**返回为void，不能是成员函数**
调用必须说明执行参数：
<<< dim3 DimGrid, dim3 DimBlock,  size_t N, cudaStream_t S >>>
N 为 每个线程块所需的动态内存字节数

_____device_____
device调用的device代码，和global互斥

_____host_____
host调用的host代码，和global互斥，但可与device并用
```cpp
__host__ __device__ func()
{
#if __CUDA_ARCH__ >= 800
   // Device code path for compute capability 8.x
#elif __CUDA_ARCH__ >= 700
   // Device code path for compute capability 7.x
#elif __CUDA_ARCH__ >= 600
   // Device code path for compute capability 6.x
#elif __CUDA_ARCH__ >= 500
   // Device code path for compute capability 5.x
#elif !defined(__CUDA_ARCH__)
   // Host code path
#endif
}
```
没有以上三个标签默认为host。
只能从host调用，_____CUDA_ARCH_____ 定义时，global、device、device+host函数调用  和  未定义时，从host调用device时都会UB。

_____noinline_____    _____forceinline_____
编译器自动inline，以上是补充提示，不能同时使用或者用于inline函数。

_____inline_hint_____
鼓励inline

#Cuda变量限定符
_____device_____
声明device上的变量。可以与后3个限定符一块用。
如果这4个都没有声明，那么：
	在全局内存区（主机）
	生命期同Cuda上下文
	各设备都有1个
	可通过运行时库（cudaGetSymbolAddress() / cudaGetSymbolSize() / cudaMemcpyToSymbol() / cudaMemcpyFromSymbol()）从网格内的所有线程和主机进行访问。

_____constant_____
在常量内存区，生命期同Cuda上下文，设备私有，可通过运行时库（cudaGetSymbolAddress() / cudaGetSymbolSize() / cudaMemcpyToSymbol() / cudaMemcpyFromSymbol()）从网格内的所有线程和主机进行访问。

_____shared_____
在线程块的共享内存区，生命期同线程块，线程块私有和访问，没有固定地址

_____grid_constant_____
用在20系显卡以上，声明const的 _____global_____ 函数参数的只读，比 const T&更严格。

_____managed_____
声明页面按需迁移的变量，应用全局生命期，device和host都能引用的对象。

_____restrict_____
受限指针，帮助编译器进行优化。“指向实际数据的第一道指针”
大致意思是：“我只会用这个指针来引用底层的数据”。
这样，编译器就可以假设不存在指针别名，从而进行各种优化。
特别是对于只读的指针，使用const * _____restrict_____ 可以提高性能。
因为如果变量是只读的，那么读取这个变量就可以被优化，因为它会被缓存。


7.3后内容用时再学