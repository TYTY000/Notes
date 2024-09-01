#同步函数 
\_\_syncthreads()
用于块内线程的同步

\_\_syncthreads_count(int predicate)
返回满足条件的线程数

\_\_syncthreads_and(int predicate)
\_\_syncthreads_or(int predicate)
返回满足条件的线程的逻辑与和或

void \_\_syncwarp(unsigned mask=0xffffffff);
等待mask绑定的所有warp都执行完此函数。

数学、纹理、表面函数都有专门的API栏。

#缓存加载
只读缓存加载
T \_\_ldg(const T* address);
这些函数是用于在具有5.0及更高计算能力的设备上进行加载和存储操作的。它们使用缓存提示来优化性能。

加载函数：

    __ldcg(const T* address): 从全局内存加载数据，但优先使用缓存。
    __ldca(const T* address): 从全局内存加载数据，避免缓存。
    __ldcs(const T* address): 从共享内存加载数据。
    __ldlu(const T* address): 从全局内存加载数据，不使用缓存。
    __ldcv(const T* address): 从常量内存加载数据。

存储函数：

    __stwb(T* address, T value): 将数据写入全局内存，并写回到L2和系统内存。
    __stcg(T* address, T value): 将数据写入全局内存，并缓存在L1和L2中。
    __stcs(T* address, T value): 将数据写入共享内存。
    __stwt(T* address, T value): 将数据写入全局内存，但不缓存。


在某些情况下，错误地使用这些函数可能会导致性能下降，而不是提高。因此，使用这些函数时应该谨慎，并确保充分理解其含义和用途。
