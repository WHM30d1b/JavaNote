## 面试题

#### 1. 作用域

①当前类 ②同一包下的类 ③子孙类 ④其他包下的类

- public：①②③④
- protected：①②③
- friendly：①②
- private：①

默认为 friendly



#### 2. 匿名内部类 Anonymous Inner Class 能否继承其他类，能否实现接口

匿名内部类适合创建只使用一次的类，没有名字，不能继承其他类，创建实例后类的定义会消失，不能重复使用

内部类可以作为一个接口，由另一个内部类实现



#### 3. Static Nested Class 和 Static Inner Class

Nested Class：嵌套类

Inner Class：内部类

区别在于是否有指向外部的引用：

静态内部类创造一个 static 的内部类对象，不需要外部类对象，且不能通过内部类对象访问到外部类对象



#### 4. && 和 &

&：按位与运算

&&：逻辑运算符 and



#### 5. assertion 断言

常用的调试方式，对 boolean 表达式进行检查，用于保证程序的正确性

assertion 检查在开发和测试时开启，在软件发布后，为了提高性能通常关闭 assertion



#### 6. Math.round(num)

num + 1/2 后求 floor



#### 7. Array 和 String 的长度

数组：length 属性

String：length() 方法



#### 8. error 和 exception

error：可以恢复但是很困难，如内存溢出，程序无法处理

exception：程序本身的问题，程序运行正常则不会出现



#### 9. 抽象类的方法能否同时是 static，native，synchronized

不能



#### 10. 接口是否能继承接口，抽象类能否实现接口，抽象类能否继承实体类

- 接口可以继承接口
- 抽象类可以实现接口
- 抽象类可以继承实体类，前提是实体类必须有明确的构造函数



#### 11. 构造器 Constructor 能否被重写

构造器不能被继承，所以也不能被重写，但可以重载 Overloading



#### 12. try{} 中有 return 语句，finally{} 中的代码何时执行

在 return 前执行



#### 13. 效率最高的方式计算 2 × 8

左移三位：2 << 3



#### 14. Java 传参

Java 语言只通过值传递参数，参数的值为对象的引用，对象本身的值可以改变，但是对象的引用不会



#### 15. switch 作用的数据类型

switch(表达式)，传递给 switch 和 case 的参数应该是 int，short，char，byte，不能为 long，string



#### 16. char 变量能否存储中文汉字

char 类型占 16 个字节，使用 unicode 编码时可以存储中文汉字



#### 17. float f = 3.4 是否正确

不正确，因为精度不准确，应该用强制类型转换

- float f = ( float )3.4
- float f = 3.4f

Java 中，没有小数点默认是 int，有小数点默认是 Double

向上可以自动转型，Double 精度高于 Float，不能自动转换



#### 18. String 和 StringBuffer

String 的长度是不可变的，StringBuffer 的长度是可变的

如果对字符串中的内容经常进行修改时，使用 StringBuffer

如果最后需要 String，使用 StringBuffer 的 toString() 方法



#### 19. final, finally, finalize

- final：修饰符
  - 被 final 修饰的类不能派生出新的子类
  - 被 final 修饰的方法只能使用，不能重载
  - 被 final 修饰的变量在声明时给初值，只能读取，不能修改
- finally：代码块，异常处理时执行清除功能的代码块
- finalize：类方法，在 GC 清除对象之前的清理工作



#### 20. 面向对象 OOP 的特征

- 抽象：忽略主题中与目标无关的方面，选择其中一部分且不注重细节
- 继承：明确表示共性，新类从现有类中派生
- 封装：把过程和数据包围起来，提供访问接口
- 多态：允许不同类的对象，对同一消息做出回应



#### 21. 异常

JAVA 程序违反语义规则时，JVM 就会将发生的错误表示为一个异常

违反语义规则包括 2 种情况：

- JAVA 类库内置的语义检查，如数组下标越界引发 IndexOutOfBoundsException
- JAVA 允许扩展语义检查，创建异常并选择在何时用 throw 关键字引发异常，所有的异常都是 java.lang.Thowable 的子类



#### 22. 排序方法

插入排序：直接插入排序，希尔排序

交换排序：冒泡排序，快速排序

选择排序：直接选择排序，堆排序

归并排序

分配排序：箱排序，基数排序



#### 23. 流

- 字节流：继承于 InputStream 和 OutputStream

- 字符流：InputStreamReader 和 OutputStreamWriter



#### 24. Java 中实现多态的机制

- 重写 Overriding：父类和子类之间的多态
- 重载 Overloading：一个类中的多态



#### 25. 是否可以从一个 static 方法内部发出对非 static 方法的调用

不可以

- 非 static 方法要与对象关联在一起，必须创建对象后才可以调用
- static 方法调用时不需要创建对象，直接调用

当一个 static 方法被调用时，可能还没有创建任何实例对象，其内部如果存在非 static 方法，无法关联到具体对象

























