类型模板、值语义
只有当子类型全部支持拷贝和移动构造时才可
std::get<index/T>(variant)   返回对应对象或者std::bad_variant_access
std::get_if<index/T>(variant)  返回对应对象指针或者nullptr

compare：
1.相同模板（包括顺序）才能比较
2.不同类型比较index
3.相同类型默认比较

如果所有的选项类型都能计算哈希值，那么 variant 对象也能计算哈希值。注意 variant 对象的哈希值不保证是当前选项的哈希值。在某些平台上它是，有些平台上不是。

赋值异常：1.可能是换类型了，肯定就没有值了，把它当做move过的状态，返回true；2.没换类型，虽然异常了，但是他还有原来的值，就是false
![[Pasted image 20240506134845.png]]
#variant多态
可以用于实现同质集合的多态（无需继承、跟踪指针等，类似虚函数，需要自行实现各类的接口函数）
```cpp
std::vector<GeoObj> figure = createFigure();
for (const GeoObj& geoobj : figure) {
std::visit([] (const auto& obj) {
obj.draw(); // 多态调用draw()
}, geoobj);
}
```
异质集合需要用类型萃取和退化的方法：
```cpp
  auto trav = [](auto &&arg) {
    using T = std::decay_t<decltype(arg)>;

    if constexpr (std::is_same_v<T, int>)
      std::cout << "值为 " << arg << " 的 int";
    else if constexpr (std::is_same_v<T, long>)
      std::cout << "值为 " << arg << " 的 long";
    else if constexpr (std::is_same_v<T, double>)
      std::cout << "值为 " << arg << " 的 double";
    else if constexpr (std::is_same_v<T, std::string>)
      std::cout << "值为 " << std::quoted(arg) << " 的 std::string";
    else
      static_assert(false, "探访器无法穷尽类型！");
  };
    for (auto &&v : vec) {
    std::visit(trav, v);
  }
```
优点有：
• 你可以使用任意类型并且这些类型不需要有公共的基类（这种方法是非侵入性的）
• 你不需要使用指针来实现异质集合
• 不需要 virtual成员函数
• 值语义（不会出现访问已释放内存或内存泄露等问题）
• vector 中的元素是连续存放在一起的（原本指针的方式所有元素是散乱分布在堆内存中的）
缺点有：
• 闭类型集合（你必须在编译期指定所有可能的类型）
• 每个元素的大小都是所有可能的类型中最大的（当不同类型大小差距很大时这是个问题）
• 拷贝元素的开销可能会更大

**一方面这种方法很安全（没有指针，意味着没有 new 和 delete），也不需要虚函数。然而另一方面，使用访问器有一些笨拙，有时你可能会需要引用语义（在多个地方使用同一个对象），还有在某些情形下并不能在编译期确定所有的类型。**

**没有了 new 和 delete可能会减少很大开销。但另一方面，以值传递对象又可能会增大很多开销。**
在实践中，必须测试代码来看哪种方法效率更高。

**如果一个 std::variant<>同时有 bool和 std::string选项，赋予一个字符串字面量可能会导致令人惊奇的事，因为字符串字面量会优先转换为 bool，而不是 std::string。**