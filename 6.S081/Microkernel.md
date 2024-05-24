#Monolithic
Big abstraction.
Pros:
1.protability. no coop
2.hide complexity. 
3.easy for resource management.
Cons:
1.too big & complex, easy to bug ,hard to debug, low security
2.not generalized, not focus, slow development.
3.hard to maintain. low extensibility.

#Microkernel
Idea: tiny kernel , inter-process communication, threads of tasks.

small -> secure , verifiable (seL4) , easier to optimize, fast( no overheads for unused features ), easier to design, more to flexible
 user level ->     more modular, easier to optimize, more robust( fatal crush)

challenges:
minimum system call API
rest of OS still need to imple
IPC speed  (RPC)20x speed , no fs(no buffer) , client send return to server recv while loop.

#L4
7 syscall : thd create , recv/ send ipc , mapping , device access , ipc intr , yield , pager handler 

13,000 lines of code
