abstract value 代表的是抽象的、理想中的数据
rep value 是实际底层的数据，你可以用多个底层数据去代表同一个不实际存在的、抽象的数据

AF  代表抽象函数，只暴露接口和行为的数据类型;
RI  代表输入需要保持不变的限制条件和性质。
representation exposure safety argument 是指抽象函数底线中是否隐藏了


不变量是一个ADT对象实例的属性，在该对象的生命周期内始终为真。  
一个好的ADT会保留自己的不变性。不变量必须由创建者和生产者建立，并由观察者和突变者保存。  
rep不变式规定了表示法的合法值，并且应该在运行时用checkRep()检查。  
抽象函数将一个具体的表示映射到它所代表的抽象值。  
表示法的暴露威胁着表示法的独立性和不变式的保存。  
不变体应该被记录下来，通过注释，甚至更好的是通过像checkRep()这样的断言，否则不变体就不会有错误。