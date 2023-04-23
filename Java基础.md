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
  - 被 final 修饰的类不能被继承派生出新的子类
  - 被 final 修饰的方法不能重写
  - 被 final 修饰的变量在声明时给初值，只能读取，不能修改
  - 被 final 修饰的方法，JVM 尝试内联提高效率
  - 被 final 修饰的常量，在编译阶段放入常量池
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



#### 26. 能否 Override 重写 static 方法

不能，static 关键字表示一个成员变量或者成员方法可以在没有类对象的情况下被访问

方法覆盖 **Override** 是**运行时动态绑定**的，而 **static** 方法是**编译时静态绑定**的



#### 27. 构造函数

当新对象被创建的时候，构造函数会被调用

- 每一个类都有构造函数，没有给类提供构造函数的情况下，Java 编译器会为这个类创建一个默认的构造函数
- 构造函数重载和方法重载很相似，可以为一个类创建多个构造函数，每个构造函数必须有唯一的参数列表
- Java 不支持像 C++中那样的复制构造函数，因为没有构造函数的情况下，Java 不会创建默认的复制构造函数



#### 28. 接口和抽象类的不同

- 包含的方法是否抽象：接口中所有的方法隐含的都是抽象的，抽象类可以同时包含抽象和非抽象的方法
- 包含的变量是否不可变：接口中声明的变量默认都是 final 的，抽象类可以包含非 final 的变量
- 类的多接口实现和单继承：类可以实现很多个接口；但是只能继承一个抽象类
- 抽象类对于接口：抽象类可以不实现抽象类和接口声明的所有方法，抽象类可以在不提供接口方法实现的情况下实现接口
- 包含的成员函数是否 public：接口中的成员函数默认是 public，抽象类的成员函数可以为 private，protected 或 public
- 能否实例化调用：接口是绝对抽象的，不可以被实例化，抽象类也不可以被实例化，但包含 main 方法的抽象类可以被调用



#### 29. 检查异常和未检查异常

- checked：编译器要求必须处置的异常，必须要用 throws 语句在方法或构造函数上声明
- unchecked：编译器不要求强制处置的异常，在运行时再检查处理



#### 30. 原始数据类型，大小，封装类

- boolean - 1/4 byte - Boolean
- byte - 1 byte - Byte
- short - 2 byte - Short
- int - 4 byte - Integer
- long - 8 byte - Long
- float - 4 byte - Float
- double - 8 byte - Double
- char - 2 byte - Character



#### 31. Object 中定义了哪些方法

- clone()
- equals()
- hashCode()
- toString()
- notify()
- notifyAll()
- wait()
- finalize()
- getClass()



#### 32. 反射

运行时，通过类的 class 对象来获取类的各种定义信息，比如属性与方法

```java
Class.forName('com.mysql.jdbc.Driver.class'); //加载MySQL的驱动类
```



实现方式：

- Class.forName(“类的路径”)
- 类名.class
- 对象名.getClass()
- 基本类型的包装类，调用包装类的 Type 属性获得 Class 对象



优点：

- 运行时动态获取类的实例，提高灵活性
- 与动态编译结合

缺点：

- 用反射需要解析字节码，将内存中的对象进行解析，性能较低，解决方案：
  - setAccessible(true) 关闭 JDK 的安全检查提升反射速度
  - 多次创建一个类的实例有缓存
  - ReflectASM 工具类，通过字节码生成的方式加快反射速度
- 通过反射可以获得私有方法和属性，破坏了封装性，相对不安全



#### 33. JDK8 新特性

1. Lambda 表达式

   - 闭包，本质上是匿名方法。允许把函数作为方法的参数，或把代码看成数据，使代码变得简洁紧凑

   - 组成：参数列表 + “—>”符号 + 函数体

   - 演示：

     ```java
     // 原始方法创建线程
     new Thread(new Runnable() {
         @Override
         public void run() {
         System.out.println("Before Java8, too much code for too little to do");
         }
     }).start();
     
     // Lambda表达式，省略中间重写方法的步骤
     new Thread( () -> System.out.println("In Java8, Lambda expression rocks !!") ).start();
     ```

2. 新的日期 API

   - 加强对日期和时间的处理，修复旧版 Java 中的问题
     - java.util.Date 非线程安全，所有日期类都是可变类
     - java.util 和 java.sql 包中都有日期类且重名，解析类又在 java.text 包中，设计不合理
     - 日期类不提供国际化时区支持
   - JDK 8 提供两个比较重要的 API：
     - Local：简化日期时间处理，没有时区问题
     - Zoned：指定时区
   - java.time 包：日期，时间，时区，时刻（instants），过程（during）与时钟（clock）的处理

3. 引入 Optional

   - 实际上是保存泛型或 null 的容器，不用显示地进行空值检测，解决空指针异常问题

4. 使用 Base64

   - 某些系统中只能使用 ASCll 字符，Base64 编码将非 ASCll 字符转换成 ASCll 字符

5. 接口的默认方法和静态方法

   - 默认方法：接口可以有实现方法，且不需要实现类去实现。在方法名前面加 default 关键字创建默认方法
   - 引进的默认方法是为了解决接口的修改与现有的实现不兼容的问题
   - 静态方法：接口可以声明，并且可以提供实现

6. 新增方法引用格式

   - 直接引用已有 Java 类或对象的方法。与lambda联合使用，使语言的构造更紧凑简洁，减少冗余代码

   - 示例：

     ```java
     // Car类
     public static class Car {
     
         // 构造器
         public static Car create( final Supplier<Car> supplier ) {
             return supplier.get();
         }                       
     
         // 静态方法
         public static void collide( final Car car ) {
             System.out.println( "Collided " + car.toString() );
         }         
     
         // 特定类，任意对象的方法
         public void follow( final Car another ) {
             System.out.println( "Following the " + another.toString() );
         }         
     
         // 特定对象的方法
         public void repair() {  
             System.out.println( "Repaired " + this.toString() );
         }
     }
     
     
     
     // 构造器引用：Class::new
     final Car car = Car.create(Car::new);
     final List<Car> cars = Arrays.asList(car);
     
     // 静态方法：Class::static_method
     cars.forEach( Car::collide );
     
     // 任意对象方法：Class::method
     cars.forEach( Car::repair );
     
     // 特定对象的方法：instance::method
     final Car police = Car.create( Car::new );
     cars.forEach( police::follow );
     ```

7. 新增 Stream 类

   - Stream 流不是一种数据结构，在原数据集上定义一组操作

   - 这些操作是惰性的，每当访问到流中的一个元素，才会在此元素上执行这一系列操作

   - 不保存数据，每个 Stream 流只能使用一次

   - 中间操作：返回结果都是Stream，可以多个中间操作叠加

     终止操作：用于返回我们最终需要的数据，只能有一个终止操作

   - 基本流程

     - 创建初始流：  

       ```
       1.Collection接口的 stream() 或 parallelStream() 方法
       
       2.静态的 Stream.of()，Stream.empty() 方法
       
       3.Arrays.stream(array, from, to)
       
       4.静态的 Stream.generate() 方法生成无限流，接受一个不包含引元的函数
       
       5.静态的 Stream.iterate() 方法生成无限流，接受一个种子值以及一个迭代函数
       
       6.Pattern 接口的 splitAsStream(input) 方法
       
       7.静态的 Files.lines(path)，Files.lines(path, charSet) 方法
       
       8.静态的 Stream.concat() 方法将两个流连接起来
       ```

     - 转换中间流：

       ```
       1.filter(Predicate):将结果为false的元素过滤掉
       
       2.map(fun)：转换元素的值，可以用方法引元或者lambda表达式
       
       3.flatMap(fun)：若元素是流，将流摊平为正常元素，再进行元素转换
       
       4.limit(n)：保留前n个元素
       
       5.skip(n)：跳过前n个元素
       
       6.distinct()：剔除重复元素
       
       7.sorted()：将Comparable元素的流排序
       
       8.sorted(Comparator)：将流元素按Comparator排序
       
       9.peek(fun)：流不变，但会把每个元素传入fun执行，用作调试
       ```

     - 聚合终结流：

       ```
       聚合：
       1.reduce(fun)：从流中计算某个值，接受一个二元函数作为累积器，从前两个元素开始持续应用它，累积器的中间结果作为第一个参数，流元素作为第二个参数
       
       2.reduce(a, fun)：a为元值，作为累积器的起点
       
       3.reduce(a, fun1, fun2)：与二元变形类似，并发操作中，当累积器的第一个参数与第二个参数都为流元素类型时，可以对各个中间结果也应用累积器进行合并，但是当累积器的第一个参数不是流元素类型而是类型T的时候，各个中间结果也为类型T，需要fun2来将各个中间结果进行合并
       
       收集：
       1.iterator()
       
       2.forEach(fun)
       
       3.forEachOrdered(fun)：应用在并行流上以保持元素顺序
       
       4.toArray()
       
       5.toArray(T[] :: new)：返回正确的元素类型
       
       6.collect(Collector)
       
       7.collect(fun1, fun2, fun3)：fun1转换流元素；fun2为累积器，将fun1的转换结果累积起来；fun3为组合器，将并行处理过程中累积器的各个结果组合起来
       
       查找与收集：
       1.max(Comparator)：返回流中最大值
       
       2.min(Comparator)：返回流中最小值
       
       3.count()：返回流中元素个数
       
       4.findFirst()：返回第一个元素
       
       5.findAny()：返回任意元素
       
       6.anyMatch(Predicate)：任意元素匹配时返回true
       
       7.allMatch(Predicate)：所有元素匹配时返回true
       
       8.noneMatch(Predicate)：没有元素匹配时返回true
       ```

8. 注解相关的改变

   - 可以在同一位置重复使用相同注解
   - 扩展了注解的适用范围

9. 支持并行（parallel）数组

   - 增加了新的方法对数组并行处理，parallelSort() 方法

10. 对并发类（Concurrency）的扩展

    - java.util.concurrent.ConcurrentHashMap 类中加入了新方法支持聚集操作
    - java.util.concurrent.ForkJoinPool 类中加入了新方法支持共有资源池（common pool）
    - java.util.concurrent.atomic 包中增加了类：
      - DoubleAccumulator
      - DoubleAdder
      - LongAccumulator
      - LongAdder



#### 34. instanceof 关键字

测试一个对象是否为一个类的实例

```java
boolean result = obj instanceof Class
```



#### 35. 基本类型和封装类型的装箱拆箱

装箱（int-->Integer）：Integer.valueOf(int) ，返回 Integer

拆箱（Integer-->int）：Integer.intValue，返回 int



#### 36. 创建对象的方式

- new 构造方法
- 反射
- clone
- 序列化



#### 37. 不相同的对象有相同 HashCode 的处理

- 拉链法：每个哈希表的节点都有 next 指针，构成单向链表
- 开放定址法：发生冲突后去寻找下一个空的散列地址
- 再哈希：用多个不同的哈希函数，发生冲突时换用下一个哈希函数直到无冲突



#### 38. +=

`+=` 操作符会进行隐式自动类型转换

- a += b 将加操作的结果类型强制转换为持有结果的类型

- a = a + b 不会自动进行类型转换



#### 39. 序列化中有些变量不想序列化

对于不想进行序列化的变量，使用 transient 关键字修饰

transient 关键字：

- 阻止实例中的变量序列化和反序列化
- 只能修饰变量，不能修饰类和方法



#### 40. IO 和 NIO ( New IO )

核心区别：

1. NIO 以块的方式处理数据，IO 以最基础的字节流的形式写入和读出，效率上 NIO 比 IO 高出很多
2. NIO 不用 OutputStream 和 InputStream 流的形式，而是基于流的形式，采用通道和缓冲区进行数据处理
3. NIO 的通道是双向的，IO 中的流是单向的
4. NIO 的缓冲区可以进行分片，建立只读缓冲区、直接缓冲区和间接缓冲区，直接缓冲区分配内存去加快 I/O 速度
5. NIO 采用的是效率高的多路复用 IO 模型，普通 IO 用的是阻塞 IO 模型

















