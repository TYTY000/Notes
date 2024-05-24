枚举类型，只能列表初始化
继承 unsigned char
可以用\` 来做数字分隔
std::byte b2{0b1111'0000};

也没有隐式类型转换，这意味着你必须显式对整数值进行转换才能初始化字节数组：
```cpp
std::byte b5[] {1}; // ERROR
std::byte b6[] {std::byte{1}}; // OK
```
![[Pasted image 20240506164140.png]]