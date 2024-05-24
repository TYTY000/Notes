```cpp
template <typename... Ts> struct overload : Ts... {
  using Ts::operator()...;
};

template <typename... Ts> overload(Ts...) -> overload<Ts...>;

auto twice = overload{[](std::string &s) { s += s; }, [](auto &v) { v *= 2; }};

int main(int argc, char *argv[]) {
  int i = 42;
  float f = 4.0;
  std::string s = "hi";
  twice(i), twice(f), twice(s);
  std::cout << i << " " << f << " " << s;
  return 0;
}
```

template<typename... Types>
class Multi : private Base<Types>...
{
public:
// 继 承 所 有 构 造 函 数：
using Base<Types>::Base...;
...
};
✝ ✆
有了所有基类构造函数的 using 声明，你可以继承每个类型对应的构造函数。
现在，当使用不同类型声明 Multi<>时：
using MultiISB = Multi<int, std::string, bool>;
你可以使用每一个相应的构造函数来声明对象：
MultiISB m1 = 42;
MultiISB m2 = std::string("hello");
MultiISB m3 = true;

在你给出的Multi类模板中，每个Multi对象都继承自它的所有基类。这意味着每个Multi对象都有其所有基类的成员变量。然而，这些成员变量并不是Multi对象自己的成员变量，而是从基类继承来的。

在你的代码中，Multi的模板参数是一组类型，每个类型都对应一个基类。因此，Multi对象的成员变量取决于这些基类。如果基类有成员变量，那么Multi对象就会有这些成员变量。

然而，需要注意的是，Multi对象并不是同时拥有所有基类的成员变量。相反，它在任何时候都只有一个基类的成员变量，这取决于你用哪个构造函数来创建Multi对象。这就是为什么你可以使用不同类型的构造函数参数来创建Multi对象的原因。

所以，虽然Multi对象可以接受多种类型的构造函数参数，但它并不像union那样在同一块内存中存储多种类型的数据。相反，它在任何时候都只有一个成员变量，这个成员变量的类型取决于你用哪个构造函数来创建Multi对象。希望这个解释对你有所帮助！