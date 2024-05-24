#clock_t
integer or floating-point type
counts of clock ticks since a certain events
#time_t
simple calendar time 
用来计算经过时间或转换成分化时间
是从1970-1-1 0:0:0开始经过的秒数，同UTC一致。
GNU C 库加了额外的符号，保证能够正确处理。
#timespec  <time.h>
用来计算了elapsed time，包含一下2个成员：
time_t  tv_sec;   秒数
long int tv_nsec;   纳秒数
#timeval  <sys/time.h>
类似timespec，但是只有毫秒级。
time_t  tv_sec;   秒数
long int tv_usec;   毫秒数
#tm
将年、月、日等时间要素分隔开。