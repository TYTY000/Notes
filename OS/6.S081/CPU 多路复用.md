1.如何实现进程间的切换？
2.如何对用户进程实现无缝切换？
3.如何保证进程组在CPU之间切换时不发生race？
4.如何使进程退出时清空堆栈？
5.要保证进程系统调用时将对应的CPU陷入内核态。
6.要能让一个yield CPU进程的进程能够被其他进程或中断唤醒。

简单但是tricky的方法：在陷入内核时把进程及其堆栈像文件一样保存起来，恢复运行时重新读取，但是这样需要每个核单独运行一个调度线程。

线程切换：保存上个进程的寄存器，恢复下个进程的寄存器，并且进行堆栈切换。
（线程和CPU中都有context，切换的时候需要关闭中断，同时要恢复CPU的中断状态，直接return到 %ra，没有%rsp）
一个想yield的进程必须获取自身的进程锁，交出所持有的其他锁，更新自身状态，并且调用调度器。

#sched
调度器负责选择下一个切换的进程。
进程调用的调度函数（实际上是进程的一部分，因为此时调度器在等待返回）需要检查进程：持有进程锁，程序状态不为RUNNING、当前CPU中断关闭。
==**实质上就是把进程锁交给了调度器。**==
调度器不会返回，他一直在：选择下一个运行的进程=》切换并运行那个进程=》等待进程返回/yield 并继续选择进程……
所以进程调用的调度函数和调度器函数是两个互相配对补齐的线程（函数），称之为协程。
调度器进行的切换动作只有在进程被fork出来第一次运行时不会进入调度函数，会直接由进程分配函数释放进程锁后陷入内核，进行文件系统的初始化。

调度器运行中，**如果进程锁锁了，就代表进程状态为RUNNING**（此时通常是时钟中断），CPU寄存器就代表了进程的寄存器，此时yield需要额外保存；
**如果进程锁空闲，代表进程状态是RUNNABLE，** 就可以让一个空闲的CPU的调度来运行，此时需要把进程中的context读取至CPU。

所以yield 的时候先要获取进程锁，并且更新进程状态，到调度函数调用完毕的时候才（由调度器）释放。调度器修改程序状态时也是同样（类似critical section）。

#sleep  这个sleep不是系统调用！这个spinlock就是条件锁。
将spinlock和事件编号作为sleep的参数，在睡觉的时候，将spinlock转换为进程锁，并且标注等待事件编号，更新SLEEPING状态，调用调度函数；在醒来的时候，清零事件编号，将进程锁转换为spinlock，即可继续进行（程序状态由wakeup更新）。

#wakeup
wakeup的只会是不上锁的进程（由前文，上锁的不是SLEEPING就是RUNNING）
只需要事件编号作为参数，对进程列表进行遍历，对不持进程锁的SLEEPING的、对应事件编号的进程**标记为RUNNABLE，不是直接运行**。（前后需要上下锁）

#pipes
pipe由buf、nwrite、nread和一个pipelock组成。
读写操作都需要acquire pipelock。
writer：如果写满了，就会wakeup reader 和 sleep /w pipelock；如果写完了，就正常wakeup reader，sleep；
reader：如果读空并且正在写，就去sleep /w pipelock（需要做running 检查，死了不让睡）；如果有的东西读，读完了wakeup writer，就去sleep。

#wait
wakeup upon child reaped
acquire wait_lock-> seek ZOMBIE child -> try copy out pagetable -> reap using exit( )   OR  sleep……
如果parent 比 child 先挂，就需要托孤给parent的ppid（init process一直在wait）

#exit
acquire process_lock-> reparent child ->set ZOMBIE, 等到ppid 的wait 发现后，再 set unused ，copy exit status, 返回 pid

**\*需要avoid race between multiple exits and exit & wait**
——我还没到wait你就exit了；突然好几个一起exit导致我处理出错等
所以需要一个wait lock， 在wait成功或者没有业务的时候才空闲。

#kill
kill another process
Simple and Stupid : set RUNNING KILLED. SLEEPING RUNNABLE **（醒来以后要检查状态！==而且和IO有联系的等延迟进度的进程可以自主退出！==）** 。

#process_lock
控制p->state , chan, killed, xstate , pid等等
**p->parent是由ppid控制的，所以需要wait_lock来锁！**
wait_lock  GLOBAL

C编辑器会保存caller 寄存器
