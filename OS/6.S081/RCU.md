concurrent readers &  writers
low computation & overhead
determinisitic completion time
核心：并行的多核共享数据结构。

单独的读写锁不能应对多读不写的情况。
改进：
```
struct rwlock {
  int n;  // n == 0 not locked, n == -1 one writer locked
          // n > 0 n readers locked
};

r_lock(l):
  while 1:
    x = l->n
    if x < 0
      continue
    if CAS(&l->n, x, x+1)
      return

w_lock(l)
  while 1:
    if CAS(&l->n, 0, -1)
      return
```
上述可以实现一读多写，但是对于写来说$O(n^2)$ 难以忍受，并且会在多核并行时造成大量cache miss，进而堵塞。
同时，并行的写一个变量（在某个核的缓存中）会导致其他核进行expensive的同步操作，导致效率极其低下。
再次，这种锁不能解决修改读写数据同步、新增、删除时指针飘飞的问题，所以引入RCU，RCU能在解决上述问题的同时保持高速性能。

1. commit: 更新指向新内容的指针（写好了再换）
2. 需要 memory barrier （因为mem可能会乱序执行）
3. 旧内容需要空闲时（延迟）释放。
上下文切换有什么用？
读：因为发生上下文切换时不允许持有RCU的指针，正在读时不能切换，切换就没有读；
写：等到每个核心都进行一次上下文切换，确保所有的写都完成了。
在synchonize_rcu( )后再free。
**同时，不让系统进行就地写入数据，而是更换数据的指针。**

### 核心思想：

- **读者** 不需要获取任何锁，只需要使用 rcu_read_lock( ) 和 rcu_read_unlock( ) 来标记临界区的开始和结束。这两个函数本质上是禁止和开启 CPU 的抢占，以保证读者不会被调度出去。读的时候返回一份copy。
- **更新者** 需要复制一份共享数据的副本，然后对副本进行修改，最后使用 call_rcu( ) 或 synchronize_kernel( ) 来注册一个回调函数，在适当的时机把原来的数据指针替换为新的数据指针，并释放旧的数据。
- **宽限期** 是指所有引用旧数据的 CPU 都退出临界区的一段时间。在宽限期结束后，回调函数才会被执行，从而完成数据的更新和销毁。宽限期是通过记录每个 CPU 的调度次数和批次号来判断的。

把CPU更新的开销分摊到调度器周期中。

![[Pasted image 20230725174138.png]]

通过RCU的优先级设置（例如比slab分配器高）和标志位的设置，可以使某块内存变成类型安全（绑定）的。