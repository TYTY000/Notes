操作系统为程序提供了执行环境，可执行文件是最重要的操作系统对象，他是一个描述了状态机的初始装填+ 迁移的数据结构。
其中有：
寄存器，大部分由ABI规定，操作系统负责设置，还有PC
地址空间，二进制文件+ABI共同决定，例如argv 和 envp 等信息的存储
其他有用的信息（如 调试 和core dump）
一个文件可执行，需要满足一些条件：
具有执行权限； ./a.c: Permission denied.
加载器可识别的可执行文件。  The file './a.c' is marked as an executable but could not be run by the operating system.

#exec_family
exec族的函数能将当前程序影响换成新的，本族的函数都基于execve，初始化参数就是要执行的文件。
这些函数按照后缀分类：
1.  **l** execl   execlp execle
![[Pasted image 20230128092932.png]]
const char \*arg 是可变参数，以要执行的文件的名开头，必须以 ( char \* ) NULL 结束
2. **v** execv execvp execvpe
![[Pasted image 20230128093313.png]]
V 代表了向量，参数以\0结束的指针数组的形式保存。
3. **e** execle  execvpe
e 代表了环境变量，没有e的函数就读取调用函数的外部环境变量env
4. p execlp  execvp  execvpe
p 代表了会用shell的搜索动作去找目标文件（文件名不包含 / ），默认从$PATH 中去寻找。如果没有 $PATH，就去找confstr(\_CS_PATH)，默认就是 /bin:/usr/bin ，也可能搜索当前目录。
execvpe 搜索 调用函数环境中的 $PATH，不从全局环境变量中找。
**如果文件名包含/，默认就是绝对路径，忽略$PATH。**
特别的，如果没有权限，就是EACCES，会继续在剩余路径中搜索；
如果文件的头无法识别，就是ENOEXEC，就会在默认路径 /bin/sh 中继续搜索（如果还FAIL，就不干了）。
不包含 p 的函数会把执行文件名作为绝对/相对路径名去执行。
****
**返回**
exec族的函数只有报错时才会返回，返回-1 和 errno 。
****
**说明**
不同系统间的$PATH可能有所区别，有的系统甚至没有 $PATH 。PATH通常包括 /bin : /usr/bin 和当前目录。
在有些系统中，当前目录会附在$PATH的最后（反木马用途）。
glibc的实现遵循传统的当前目录在$PATH开头的传统。

execlp 和 execvp 出错时的行为，BSD会延后重新执行，linux会立即报错返回。

#errno
\# include <errno.h>
errno 代表程序出错类型，**调用成功的函数可以修改errno，errno从来不会被任何系统调用或库函数设为 0** 。
对于部分系统调用和库函数 会把 -1作为合法的成功返回值。此时看errno是否为 0 即可。
errno不能被显式定义，有可能是宏，是线程本地参数，线程间互不相关。
****
linux上可以通过errno命令查看报错的种类。
****

#execve
argv 和 envp 会直接输入到程序的main函数中执行，但是envp是从外部环境变量environ中取得的。
*如果程序正在被ptrace，当成功执行时，会返回SIGTRAP信号。*
当文件设置属主ID位set时，调用的程序的有效属主ID也会顺延，除非以下情况：
1.调用程序的no_new_privs 参数被set；
2.底层文件系统被nosuid方式挂载；
3.当前文件被ptrace。
并不会改变进程原本的UID和GID、以及相关的补充属组ID。
如果要执行的是个a.out动态链接共享库的二进制执行文件，linux的动态链接器 ld.so 就会在执行前被调用，将所需库内容复制进内存并且链接。
如果要执行的是动态链接的ELF可执行文件，一个在PT_INTERP代码段中的翻译器就会来加载所需库对象，这个翻译器就是用来将二进制文件链接至glibc的ld-linux.so.2。
#execve对程序属性的影响
重置被捕获的信号；
丢弃信号栈；
重置内存映射；
解绑System V的共享内存段；
解绑POSIX共享内存区域；
关闭POSIX开放的消息队列描述符；
关闭开启的POSIX信号量；
丢弃POSIX计时器；
关闭开启的目录流；
丢弃内存锁；
注销退出处理程序；
重置浮点数环境。

#execve的errors
E2BIG：参数的容量太大
EACCES：不可执行；缺少x权限；文件系统noexec挂载
EAGAIN：改了UID，导致进程超出了新的UID的资源限制。内核会设置一个PF_NPROC_EXCEEDED标记，让调用报错EAGAIN。fork()+setuid()+execve()，如果成功fork或execve，就会清空这个标记。
EFAULT：argv和envp的指针指向了无权限的非法地址。
EINVAL：一个ELF文件有多个PT_INTERP段（尝试多个翻译器）
EIO：IO故障
EISDIR：试图执行一个目录
ELIBBAD：文件格式无法识别
ELOOP：达到了最大递归次数限制；解析文件名时遇到了多个软链接。
EMFILE：同时操作某fd的进程数达到上限。
ENAMETOOLONG：路径名太长
ENFILE：系统层面fd数量达到上限
ENOENT：文件不存在
ENOEXEC：可执行文件（可是+x的）格式无法识别，要不架构错误，要不格式错误导致无法执行
ENOMEM：内核内存不足
ENOTDIR：路径名中的前缀不是目录
EPERM：一个没有功能的应用不能得到可执行文件赋予的全部功能
ETXTBSY：目标执行文件同时被多写
****
\# **NOTES**
设置了属主ID和属组ID的进程不能被ptrace

nosuid挂载的文件系统，有的在EPERM时不会执行，有些会忽略设置的属主ID和属组ID直接执行

linux上argv 和 envp可以为NULL，直接就是 NULL,但是不要滥用这个不可移植的和非标准的特性，在某些Unix系统上，这会导致EFAULT。

通常，execve  fail时，控制会移交回原程序镜像，进而处置fail error。在某些场合，可能会因为过了某些时间点而失败：例如当原程序镜像已经被撤销，新的镜像就不能正确的产生，此时内核会直接给SIGSEGV或者SIGKILL信号杀掉这个进程。

内核会对脚本开头的 “#！”实施长度限制，超出即被忽略。Linux5.1后有255个字符；Linux中，在翻译器名字后的整个字符串都被作为一个参数传递给翻译器；但是有的系统会把空格作为字符串结束符，因为这个系统的解释器可以有多个参数（可变参数）。

linux 和 大部分Unix系统都会在脚本中忽略 属主ID和属组ID。