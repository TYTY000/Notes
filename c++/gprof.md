1.正常编译  需要-pg选项
gcc -Wall -pg test_gprof.c test_gprof_new.c -o test_gprof
2.正常执行文件
./test_gprof 
会默认产生gmon.out
3.运行gprof
gprof test_gprof gmon.out > analysis.txt
4.查看生成文件
默认有两栏，前者是总览，后者是调用图  后紧跟说明文字 ，可用-b去除
用-p -P 单独显示、抑制总览
用-q 显示调用图 -Q抑制
可以用-p/P/q*    \*为函数名  只显示该函数的调用情况
可以用-a 屏蔽静态函数
