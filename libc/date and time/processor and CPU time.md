在优化程序和评价效率的时候，这两个时间很有用。
此时，日历时间和经过时间都没有用，因为可能被堵塞没有实际运行。

CPU time 是CPU周期数计数，类型clock_t，自从程序创建时就开始计数，可以通过完成计算前后的时间来计算出所用的processor time...
在linux 系统上，clock_t在整数值上面等同于 long int ,  CLOCKS_PER_SEC。
建议在操作时将 CPU time 转换为double，能保证计算、print等操作不会出问题。

#clock   
\#include <time.h>

clock_t start, end;
double cpu_time_used;

start = clock();
… /* Do the work. \*/
end = clock();
cpu_time_used = ((double) (end - start)) / CLOCKS_PER_SEC;

#times   <sys/time.h>
times函数以tms对象的形式返回一个进程消耗的处理器时间。
struct tms {
clock_t tms_utime;  //总消耗时间
clock_t tms_stime;  //系统代表进程消耗的时间
clock_t tms_cutime;   //子进程的时间，但不包括wait 和waitpid 没报告的子进程。
clock_t tms_cstime;  //同上，系统代表进程消耗的子进程时间。
...
}
Function: _clock_t_ **times** _(struct tms *buffer)_