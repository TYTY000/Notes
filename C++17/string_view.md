char\* + len
总结起来就是小心地使用 std::string_view，也就是说你应该按下面这样调整你的编码风格：
• 不要在那些会把参数传递给 string 的 API 中使用 string view。
– 不要用 string view 形参来初始化 string 成员。
– 不要把 string 设为 string view 调用链的终点。
• 不要返回 string view。
– 除非它只是转发输入的参数，或者你可以标记它很危险，例如，通过命名来体现危险性。
• 函数模板永远不应该返回泛型参数的类型 T。
– 作为替代，返回 auto类型。
• 永远不要用返回值来初始化 string view。
• 不要把返回泛型类型的函数模板的返回值赋给 auto。
– 这意味着 AAA(总是 auto(Almost Always Auto)) 原则不适用于 string view。
如果因为这些规则太过复杂或者太困难而不能遵守，那就完全不要使用 std::string_view（除非你知道自己
在做什么）。

string的操作  sv都有，sv还有remove_suffix/prefix。
C++ 标准库保证值相同的字符串和字符串视图的哈希值相同。

• 它并不需要结尾有空字符。给一个以单个 const char* 为参数而没有长度参数的 C 函数传递参数时就不属于
这种情况。
• 它不会违反传入参数的生命周期。通常，这意味着接收函数只会在传入值的生命周期结束之前使用它。
• 调用者函数不应该更改底层字符的所有权（例如销毁它、改变它的值或者释放它的内存）。
• 它可以处理参数值为 nullptr的情况
• 不要把临时字符串赋给字符串视图。 
• 不要返回字符串视图。
• 不要在调用链中使用字符串视图来初始化或重设字符串的值。


Things to remember about std::string:

    Initializing and copying std::string is expensive, so avoid this as much as possible.
    Avoid passing std::string by value, as this makes a copy.
    If possible, avoid creating short-lived std::string objects.
    Modifying a std::string will invalidate any views to that string.

Things to remember about std::string_view:

    std::string_view is typically used for passing string function parameters and returning string literals.
    Because C-style string literals exist for the entire program, it is always okay to set a std::string_view to a C-style string literal.
    When a string is destroyed, all views to that string are invalidated.
    Using an invalidated view (other than using assignment to revalidate the view) will cause undefined behavior.
    A std::string_view may or may not be null-terminated.