一个文件系统要支持：
1.使磁盘上的特定数据结构，支持路径树和文件的显示，保存含有文件内容的块的标识，支持记录空闲区域。
2.支持文件的崩溃恢复。
3.支持进程的并发处理文件。
4.支持内存中常用块的缓存。

xv6:
write-ahead-rule：写数据之前要先写元数据（宣告摇动的数据）
freeing-rule：当一条log的所有操作都完成之后才能重用这个log区域（直接擦掉头块中的块序号即可，类似bitmap）

ext3：
#journaling
journal 可以序列写入磁盘，减少IO；
可以从内存中不占用CPU直接写入磁盘。

linux中有描述块指向了journal metadata block，都是顺序写入，需要保持两个指针，head 和 tail（最老的、没有被探测的log块）。如果没有空间了就延迟到清理end指针后写入。
journal文件在固定位置包含了头数据的块，记录了当前head，tail还有一个序列数，在恢复时，从tail向head依次扫描log的头块，获取带有最高序列数的块。

#committing_the_journal
当距离上次commit时间够长或者scarce of journal space时，就要commit filesystem updates了。
当一个复杂的transaction 完成后，在没有commit 到disk 时，还需要跟踪记录transaction元数据缓冲区。当commit a transaction 时，新的文件块还在journal中，没有被写到磁盘上（为了防止在commit前 crash，需要保存旧数据）。一旦journal commited，disk上旧版本的文件就没用了，可以在空闲时间写回。直到完成写回前，都不能删除journal。
1.关闭系统中断
2.等待未完成的transaction。
3.开启新的transaction
4.写描述块，更新journal头块，更新 head 和 tail 指针，写入磁盘。
5.写log块
6.等待所有的transaction journal写入。
7.写commit
8.等commit写完
9.写入磁盘家目录。
10.重用log块。把buffer更新入journal，称为 pin，如果写入磁盘，就是 unpin 。当一个 transaction 的最后的buffer unpin ，才能重用这个journal块。

冲突：
前后写存在依赖关系，要防止错误的覆写。
可以拷贝一个新的buffer给后出现的transaction用，当commit后再删除。

|——————|——————|——————|—————|
|                      |   MAGIC No |     DATA        |   MAGIC    |
|   OFFSET       |     BLOCK \#  |    BLOCK       |  commit    |
|    SEQ No.     |                     |       .  \* n       |                   |
|——————|——————|——————|—————|
                       .....................................\* n