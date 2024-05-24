不是模板类
值语义
因为可能会使用堆内存，所以拷贝 std::any的开销一般都很大。**更推荐以引用传递对象，或者 move 值。** std::any支持部分 move 语义。
a.type() == typeid(int)
为了访问内部的值，你必须使用 std::any_cast<>将它转换为真正的类型：
auto s = std::any_cast<std::string>(a);
如果不需要初始化其他变量，更推荐转换为引用类型来避免创建临时对象：
auto s = std::any_cast<const std::string&>(a);
转换失败时会抛出std::bad_any_cast

no hash  no compare
![[Pasted image 20240506134823.png]]