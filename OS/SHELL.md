#管道调用
管道把两个文件描述符分别绑定到写口读口，然后会fork两次，分别把stdin、stdout绑定到读口和写口，关闭子进程的两个文件描述符，然后清空两个文件描述符，分别等待两个子进程结束。
#命令的顺序结构
cmd1&&cmd2 ：一直执行到失败为止（失败后边的内容就被丢弃了）
cmd1||cmd2   ：一直执行到成功为止（成功后边的内容也被丢弃了）
cmd1&cmd2  ：并行执行
cmd1；cmd2 ：顺序执行
#linux的脚本变量
linux脚本运行时常会用到一些变量，大致如下：

$$：Shell本身的PID（ProcessID）
$!：Shell最后运行的后台Process的PID
$?：最后运行的命令的结束代码（返回值）
$-：使用Set命令设定的Flag一览
$*：所有参数列表。用”$1” “$2” … “$n” 来获取参数值。
$@：所有参数列表。用”$1” “$2” … “$n” 来获取参数值。
$#：添加到Shell的参数个数
$0：Shell本身的文件名
$1～$n：添加到Shell的各参数值。$1是第1参数、$2是第2参数…。
e.g.:#!/bin/bash

echo “File Name: $0”;

echo “First Parameter : $1”;

echo “First Parameter : $2”;

echo “Quoted Values: $@”;

echo “Quoted Values: $*”;

echo “Total Number of Parameters : $#”

运行结果：
$./test.sh Zara Ali
File Name : ./test.sh
First Parameter : Zara
Second Parameter : Ali
Quoted Values: Zara Ali
Quoted Values: Zara Ali
Total Number of Parameters : 2


