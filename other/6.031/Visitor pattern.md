Java、C++没有Pattern matching，需要借助功能接口，在接口的实现子类中调用各类的函数，在各类的函数中进行不同的实现。
即：把要实现递归调用的函数设计为接口类，分别调用各类中的子函数，实际上是递归调用。

  
The Visitor pattern overcomes a missing feature in the Java language, giving us a type-safe way to represent and implement functions over a recursive data type as self-contained modules. The visitor instances are also functional objects, so we can naturally pass them around our program as values.

If we expect to add new operations to our recursive ADT in the future, using visitor makes our code ready for change without sacrificing the safety from bugs provided by static checking.

If we are building a _little language_ where the ADT is designed for the purpose of allowing clients to define their own operations on our type from outside our implementation classes, visitor makes this possible.