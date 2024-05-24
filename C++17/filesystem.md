```cpp
namespace fs = std::filesystem;
fs::path p{argv[1]}; 
fs::status(p).type()
````
```cpp
enum fs::file_type {regular, directory, not_found, symlink, unknown, block, character, fifo, socket, none}
```
path重载了/   /=
可以进行比较，操作符只比较路径字符串，可以p.lexically_normal()后进行比较。

```cpp
bool pathsAreEqual(const std::filesystem::path& p1,
const std::filesystem::path& p2)
{
return exists(p1) && exists(p2) ? equivalent(p1, p2)// equivalent需要两个都存在
: p1.lexically_normal() == p2.lexically_normal();
}
``` 
**非成员函数都是要调用文件系统的**
#文件属性

![[Pasted image 20240507105722.png]]
这些函数（除了 is_symlink()之外）都会解析符号链接。
**也就是说，对于一个指向目录的符号链接， is_symlink()和 is_directory()都返回 true**

![[Pasted image 20240507105942.png]]
注：都是非成员函数，漏了

#文件状态
为了避免文件系统访问，有一个特殊的类型 file_status可以被用来存储并修改被缓存的文件类型和权限。
发生以下情况时可以设置状态：
• 当使用表文件状态的操作中的方法访问某个指定路径的文件状态时
• 当遍历目录时
![[Pasted image 20240507112103.png]]![[Pasted image 20240507112232.png]]


![[Pasted image 20240507152926.png]]
核心是path，附带的一些基本操作，需要注意可移植性，还有注意remove。