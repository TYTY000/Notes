重点：
1. 实现对进程vma表的遍历
2. 注意flags中的写回和prot的可写是绑定的，对应页表的可写；prot的可读对应页表的可读，注意在处理页表到文件转换时要对应
3. fork的时候要拷贝父进程的vma表
4. exit的时候要删除vma表
5. 如果没有读完，减去相应长度
6. lazy allocation就是在发现页表PTE_V时 不做任何处理  continue。
7. 注意r_scause  和  stval的处理