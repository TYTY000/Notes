常见语句：
```c++
cmake_minimum_required(VERSION 3.15)
project(Tutorial VERSION 1.0)
set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU,LCC>") // 编译条件判断表达式$<var:v1>
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>") // 可以嵌套、命名${}使用
configure_file(TutorialConfig.h.in TutorialConfig.h) //编译输入生成文件
# add the MathFunctions library
add_subdirectory(MathFunctions)
# add the executable
add_executable(Tutorial tutorial.cxx)

# 接口库 specify the C++ standard
add_library(tutorial_compiler_flags INTERFACE)
target_compile_features(tutorial_compiler_flags INTERFACE cxx_std_11)

target_link_libraries(Tutorial PUBLIC MathFunctions tutorial_compiler_flags)

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

```

#逻辑判断
```c++
# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON) // 有点类似作为设置项的宏
if (USE_MYMATH)
  target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")
  add_library(SqrtLibrary STATIC mysqrt.cxx )
  target_link_libraries(SqrtLibrary PUBLIC tutorial_compiler_flags) // 链接到sqrt库都要按照tutorial_compiler_flags编译
  target_link_libraries(MathFunctions PRIVATE SqrtLibrary)  // 只有MathFunctions 按照 SqrtLibrary来编译
endif()
```

#install
```c++
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h" DESTINATION include)
```
> [!NOTE] Title
> 
需要注意，这是安装路径，build文件夹下，还是原结构不变。
* Linux默认安装路径：
    Release：默认安装路径是 /usr/local。
    Debug：默认安装路径也是 /usr/local，除非在CMakeLists.txt中指定了不同的路径。
* –prefix指定路径：
    使用 --prefix 选项可以将文件安装到指定的文件夹。例如：
    cmake --install . --prefix /my/install/prefix
    这会将文件安装到 /my/install/prefix。
* Windows默认安装路径：
    默认情况下，CMake会将文件安装到 C:/Program Files/${PROJECT_NAME}。如果没有指定 --prefix，则会安装到当前项目文件夹

#Test
```c++
enable_testing() # enabling
# -- or --
include(CTest) # 搭配CTest模块使用，详见后文
add_test(NAME Runs COMMAND Tutorial 25)
set_tests_properties(Usage PROPERTIES PASS_REGULAR_EXPRESSION "Usage.*number")
```
#function 
类似C中的宏，用来批量生成指令。
```c++
function(do_test target arg res)
    add_test(NAME "Comp_${arg}" COMMAND ${target} ${arg}
        set_tests_properties(Comp${arg} PROPERTIES PASS_REGULAR_EXPRESSION ${res}))
endfunction()
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```
#CTest
```c++ 
## CTestConfig.cmake
set(CTEST_PROJECT_NAME "CMakeTutorial") // 模块名
set(CTEST_NIGHTLY_START_TIME "00:00:00 EST") // 设置开始时间
# 以下是测试结果文档的发送地址
set(CTEST_DROP_METHOD "http")
set(CTEST_DROP_SITE "my.cdash.org")
set(CTEST_DROP_LOCATION "/submit.php?project=CMakeTutorial")
set(CTEST_DROP_SITE_CDASH TRUE)
```

> [!NOTE] 调用方法
> 调用ctest [-VV] -D Experimental     后
> 测试结果会发送到https://my.cdash.org/index.php?project=CMakeTutorial.
> 这个是公开可见的。

![[Pasted image 20240901131715.png]]

#判断系统依赖
>[!NOTE] 检查编译情况
```c++
// 添加检查变量 简短的代码即可
  check_cxx_source_compiles("
  #include <cmath>
  int main(){
  std::log(1.0);
  return 0;
  }
  " HAVE_LOG) 
  check_cxx_source_compiles("
  #include <cmath>
  int main(){
  std::exp(1.0);
  return 0;
  }
  " HAVE_EXP) 

	if(HAVE_LOG AND HAVE_EXP)
        target_compile_definitions(SqrtLibrary PRIVATE "HAVE_EXP" "HAVE_LOG")// 给这个库加宏
    endif()
```
#安装包

```c++
include(InstallRequiredSystemLibraries)
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set(CPACK_PACKAGE_VERSION_MAJOR "${Tutorial_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${Tutorial_VERSION_MINOR}")
set(CPACK_SOURCE_GENERATOR "TGZ")
include(CPack)
```
然后在编译文件夹中调用
```c++
cpack #只生成安装包
cpack -G ZIP -C Debug # 可在cpack --help中查看支持的压缩类型
cpack --config CPackSourceConfig.cmake   # 生成源码
```

#动态库
```c++
# 声明动态库调用
#if defined(_WIN32)
#  if defined(EXPORTING_MYMATH)
#    define DECLSPEC __declspec(dllexport)
#  else
#    define DECLSPEC __declspec(dllimport)
#  endif
#else // non windows
#  define DECLSPEC
#endif

namespace mathfunctions {
double DECLSPEC sqrt(double x);
}

# 在主cmake中定义变量和开关
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")

option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

# 在子cmake中定义PIC
  set_target_properties(SqrtLibrary PROPERTIES POSITION_INDEPENDENT_CODE ${BUILD_SHARED_LIBS} || ON)
```

#调试发布版本
```c++
# 设置后缀名
set(CMAKE_DEBUG_POSTFIX d)
add_library(tutorial_compiler_flags INTERFACE)
................;
add_executable(Tutorial tutorial.cxx)
# 设置调试区分版本
set_target_properties(Tutorial PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})
target_link_libraries(Tutorial PUBLIC MathFunctions tutorial_compiler_flags)
set_property(TARGET MathFunctions PROPERTY VERSION "1.0.0")
set_property(TARGET MathFunctions PROPERTY SOVERSION "1")

```

```bash
新建文件夹
- Step12
   - debug
   - release
cd debug
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build .
cd ../release
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```

```c++
双重打包
----  MultiCPackConfig.cmake

include("release/CPackConfig.cmake")

set(CPACK_INSTALL_CMAKE_PROJECTS
    "debug;Tutorial;ALL;/"
    "release;Tutorial;ALL;/"
    )
```
```bash
cpack --config MultiCPackConfig.cmake

```
TODO: Step 11: Adding Export Configuration