1.隐式推导就是根据目标文件的类型（例如.o或.exe），找到相应的依赖文件（例如.c或.cpp），然后应用相应的编译规则。
2.C++ 版本和gnu版本的对应关系？
3.cc -M会生成所有依赖，包括标准库中的头文件；而cc -MM只会生成用户代码中的依赖，不包括标准库。
4.这里，我们给出了一个模式规则来产生 .d 文件：
``` makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
    sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
    rm -f $@.$$$$
```
这个规则的作用是为每个.c文件生成一个对应的.d文件，这个.d文件包含了.c文件的依赖关系。
这行命令会生成所有的依赖关系；
这行命令会将依赖关系转换为make可以识别的格式；
会删除临时文件。
5.@就是显式指定命令，不要显示。

#显示命令
make ............... -n可以显示对应命令，可用于调试

#先后顺序
**make命令的先后顺序需要在同一行用分号前后表示**。

#命令出错
有些时候，命令的出错并不表示就是错误的。
为了做到这一点，忽略命令的出错，我们可以在Makefile的命令行前加一个减号 - （在Tab键之后），标记为不管命令出不出错都认为是成功的。如：
```
clean:
    -rm -f *.o
```
还有一个全局的办法是，给make加上 -i 或是 --ignore-errors 参数，那么，Makefile中所有命令都会忽略错误。而如果一个规则是以 .IGNORE 作为目标的，那么这个规则中的所有命令将会忽略错误。这些是不同级别的防止命令出错的方法，你可以根据你的不同喜欢设置。

还有一个要提一下的make的参数的是 -k 或是 --keep-going ，这个参数的意思是，如果某规则中的命令出错了，那么就终止该规则的执行，但继续执行其它规则。

#嵌套make
例如，我们有一个子目录叫subdir，这个目录下有个Makefile文件，来指明了这个目录下文件的编译规则。那么我们总控的Makefile可以这样书写：
```
subsystem:
    cd subdir && $(MAKE)
```
其等价于：
```
subsystem:
    $(MAKE) -C subdir
```
定义$(MAKE)宏变量的意思是，也许我们的make需要一些参数，所以定义成一个变量比较利于维护。这两个例子的意思都是先进入“subdir”目录，然后执行make命令。

我们把这个Makefile叫做“总控Makefile”，总控Makefile的变量可以传递到下级的Makefile中（如果你显示的声明），但是不会覆盖下层的Makefile中所定义的变量，除非指定了 -e 参数。

如果你要传递变量到下级Makefile中，那么你可以使用这样的声明:
```
export <variable ...>;
```
如果你不想让某些变量传递到下级Makefile中，那么你可以这样声明:
```
unexport <variable ...>;
```
需要注意的是，有两个变量，一个是 SHELL ，一个是 MAKEFLAGS ，这两个变量不管你是否export，其总是要传递到下层 Makefile中，特别是 MAKEFLAGS 变量，其中包含了make的参数信息，如果我们执行“总控Makefile”时有make参数或是在上层 Makefile中定义了这个变量，那么 MAKEFLAGS 变量将会是这些参数，并会传递到下层Makefile中，这是一个系统级的环境变量。


如果你定义了环境变量 MAKEFLAGS ，那么你得确信其中的选项是大家都会用到的，如果其中有 -t , -n 和 -q 参数，那么将会有让你意想不到的结果，或许会让你异常地恐慌。

还有一个在“嵌套执行”中比较有用的参数， -w 或是 --print-directory 会在make的过程中输出一些信息，让你看到目前的工作目录。比如，如果我们的下级make目录是“/home/hchen/gnu/make”，如果我们使用 make -w 来执行，那么我们会看到:
```
make: Entering directory `/home/hchen/gnu/make'.

make: Leaving directory `/home/hchen/gnu/make'
```
#定义命令包
类似宏，注意字母。
```
define run-yacc
yacc $(firstword $^)
mv y.tab.c $@
endef
```

#定义 
=   表示可不初始化的定义，在后文定义
:=  表示初始化定义，必须在此定义
?= 表示如果未定义，那就定义

#其他用法
```
foo := a.o b.o c.o
bar := $(foo:.o=.c)   等价于   bar := $(foo:%.o=%.c)
```
-----------------------------------------------------------
```
a_objects := a.o b.o c.o
1_objects := 1.o 2.o 3.o

sources := $($(a1)_objects:.o=.c)
```
--------------

```
libs_for_gcc = -lgnu
normal_libs =

ifeq ($(CC),gcc)
    libs=$(libs_for_gcc)
else
    libs=$(normal_libs)
endif

foo: $(objects)
    $(CC) -o foo $(objects) $(libs)
```
#隐含规则一览

**编译C程序的隐含规则。**
\*.o 的目标的依赖目标会自动推导为 \*.c ，并且其生成命令是 $(CC) –c $(CPPFLAGS) $(CFLAGS)
**编译C++程序的隐含规则。**
\*.o 的目标的依赖目标会自动推导\*n.cc 或 \*.cpp 或是 \*.C ，并且其生成命令是 $(CXX) –c $(CPPFLAGS) $(CXXFLAGS) 。（建议使用 .cc 或 .cpp 作为C++源文件的后缀，而不是 .C ）

\* 目标依赖于 \*.o ，通过运行C的编译器来运行链接程序生成（一般是 ld ），其生成命令是： $(CC) $(LDFLAGS) \*.o $(LOADLIBES) $(LDLIBS) 。这个规则对于只有一个源文件的工程有效，同时也对多个Object文件（由不同的源文件生成）的也有效。例如如下规则:

x : y.o z.o

并且 x.c 、 y.c 和 z.c 都存在时，隐含规则将执行如下命令:

cc -c x.c -o x.o
cc -c y.c -o y.o
cc -c z.c -o z.o
cc x.o y.o z.o -o x
rm -f x.o
rm -f y.o
rm -f z.o


#伪目标
.INTERMEDIATE  中间文件，完成后删除
.SECONDARY      二级文件，不删除
.PRECIOUS          珍贵文件，出错也不删除
.SUFFIXES
.SUFFIXES:              # 删除默认的后缀
.SUFFIXES: .c .o .h   # 定义自己的后缀

#自动变量
$<是依赖  $@是目标
$%仅当目标是函数库文件中，表示规则中的目标成员名。例如，如果一个目标是 foo.a(bar.o) ，那么， $% 就是 bar.o ， $@ 就是 foo.a 。如果目标不是函数库文件（Unix下是 .a ，Windows下是 .lib ），那么，其值为空。
$^依赖目标set
$?所有比目标新的依赖目标的集合
$+所有依赖目标的multiset
$\*这个变量表示目标模式中 % 及其之前的部分。如果目标是 dir/a.foo.b ，并且目标的模式是 a.%.b ，那么， $* 的值就是 dir/foo 。这个变量对于构造有关联的文件名是比较有效。如果目标中没有模式的定义，那么 $* 也就不能被推导出，但是，如果目标文件的后缀是make所识别的，那么 $* 就是除了后缀的那一部分。

#隐含规则搜索算法


#函数库文件
你可以使用“后缀规则”和“隐含规则”来生成函数库打包文件，如：

.c.a:
    $(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $*.o
    $(AR) r $@ $*.o
    $(RM) $*.o

其等效于：

(%.o) : %.c
    $(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $*.o
    $(AR) r $@ $*.o
    $(RM) $*.o

在进行函数库打包文件生成时，请小心使用make的并行机制（ -j 参数）。如果多个 ar 命令在同一时间运行在同一个函数库打包文件上，就很有可以损坏这个函数库文件。所以，在make未来的版本中，应该提供一种机制来避免并行操作发生在函数打包文件上。

但就目前而言，你还是应该不要尽量不要使用 -j 参数。