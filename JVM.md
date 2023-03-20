## 1. JVM基础

原始文件 x.java

执行 javac 命令 =》x.class

==JVM：==

执行 java 命令 =》ClassLoader 加载到内存，同时加载用到的 java 类库

调用字节码解释器（解释）或 JIT 即时编译器（编译）

执行引擎（执行）面对OS和硬件

![1677030118103](C:\Users\18378\AppData\Roaming\Typora\typora-user-images\1677030118103.png)



#### 问题1 java是解释执行的还是编译执行？

解释和编译是可以**混合的**，JVM会把特定常用代码的即时编译做成一个本地编译，就像C语言在windows上执行的时候把它编译成exe一样，下次再执行这段代码的时候就不需要解释器来一句一句的解释来执行，执行引擎可以直接交给操作系统去让他调用，效率会高很多。不是所有的代码都会被 JIT 进行及时编译的。如果是这样的话，整个java就变成了不能跨平台了。





JVM（Java Virtual Machine）：跨语言的平台（ scala，kotlin，groovy，clojure，jython，jruby 等上百种）

- 建立在 OS 和语言之间，对多种语言的执行屏蔽 OS 底层

java：跨平台的语言



#### 问题2 JVM 和 java 的关系

JVM 和 java **无关**。JVM 是一种规范，只要能编译成 .class 格式，符合 class 规范的文件都能执行。





JVM 规范

-java virtual machine specifications

-https://docs.oracle.com/en/java/javase/13/ 

-https://docs.oracle.com/javase/specs/index.html



JVM 是一种规范，有多个版本的实现。

- Hotspot : Oracle官方版，用的最多的 java 虚拟机。在命令行里写：java -version 会输出虚拟机的名字，执行模式，版本等信息。

  ```powershell
  # HotSpot(TM) 64位的 Server 版 执行模式是mixed mode
  C:\Users\18378>java -version
  java version "1.8.0_333"
  Java(TM) SE Runtime Environment (build 1.8.0_333-b02)
  Java HotSpot(TM) 64-Bit Server VM (build 25.333-b02, mixed mode) 
  ```

- Jrockit : 曾经号称世界上最快的 JVM，后来被Oracle收购，合并于hotspot。

- J9 -IBM : IBM 自己的 java 虚拟机的实现，名字叫J9。

- Microsoft VM : 微软自己的实现，都是符合虚拟机规范的。

- TaobaoVM ：淘宝自己的VM，实际上是hotsopt的定制版，专门为淘宝准备的，阿里、天猫都是用的这款虚拟机。

- LiquidVM : 直接针对硬件的虚拟机 VM，下面是没有操作系统的，直接就是硬件，运行效率更高。

- azul zing : 很贵的商业产品。既然这样就一定有自己的特点：快、非常快，尤其是垃圾回收在1毫秒之内，是业界标杆。他的一个垃圾回收的算法后来被Hotpot吸收才有了现在的 ZGC 。



#### 问题3 JDK - JRE - JVM

JVM：java 虚拟机，只负责执行

JRE = JVM + corelib：java 运行时环境，虚拟机和 java 的核心类库

JDK = JRE + development kit：java开发工具包，包含了JRE和JVM。





## 2. Class文件格式

实质上是以8位字节为基础单位的二进制流

构成详解：

- 《Java虚拟机规范（第二版）》
- 《深入理解Java虚拟机》

https://cloud.tencent.com/developer/article/1447268

https://cloud.tencent.com/developer/article/1447269

构成部分：

- Magic Number 代表 class 的版本号

- Minor Version (小号) / Major Version (主号）

- constant_pool_cont (有多少个常量池)

- 常量池-1具体的内容有很多种类型

- access_flags（class前面写的是public还是private等关键字）

- this_class (我当前的这个类是谁)

- super_class (父类是谁) 

- Interfaces_count (实现了哪些接口）

- Interfaces (接口的索引）

- fields (有哪些属性）

  access_flags ：是不是public的，是不是static的，是不是final的？？？

  name_index：名称的索引

  descriptor_index：描述符，到底是什么类型的

  attributes：另外它所附加的一些属性

- methods (方法的各种结构，到底是怎么样进行标识的，它名字的索引、描述符的索引、附加属性)



#### 问题1 JVM的每个指令在多线程是原子的吗？

JVM有8个指令是原子性。

- lock（锁定）
- read（读取）
- load（载入）
- use（使用）
- assign（赋值）
- store（存储）
- write（写入）
- unlock（解锁）



## 3. ClassLoader类加载器

class 文件从硬盘到内存准备执行

- 第一大步叫 Loading，将二进制文件装载到内存中

- 第二大步叫 Linking

  Linking 又分为三小步

  - 第一小步 Verification，校验 class文件是否符合标准
  - 第二小步 Preparation，静态变量赋默认值
  - 第三小步 Resolution，把引用转换成内存地址，变成可以直接访问的内容

- 第三大步叫 Initlalizing，静态变量赋初始值



类加载器 ClassLoader 内容，JVM 双亲委派机制

Custom ClassLoader 父类加载器 > application 父类加载器 > Extension 父类加载器 > Bootstrap（顶层）

- Bootstrap：C++实现，加载核心类。调用 getClassLoader() 拿到加载器的结果是一个空值的时候代表已经到达了最顶层的加载器。

- Extension：加载扩展 jar 包，在jdk安装目录jre/lib/ext下的jar。

- App：加载 classpath 指定内容

- Custom ClassLoader：自定义的加载器

  查看class被哪个加载器装载到内存中

  ```java
  System.out.println(String.class.getClassLoader());
  ```

加载到内存后创建两块内容，第一块内容 x.class 二进制，第二块内容生成 x.class 类的对象并指向第一块内容



#### 问题1 双亲委派机制

自底向上，检查该类是否加载，parent方向

自顶向下，进行实际查找和加载，child方向

底层的缓存没有找到类，向上反馈并委托查询；顶层反馈没有找到类，向下委派加载类。

为了**安全**。如果你也写了一个 java.lang.String 类，那么JVM只会按照上面的顺序加载jdk自带的String类，而不是你写的String类。还能保证同一个类不会被加载多次。





自定义类加载器：

- **继承** ClassLoader
- **重写**模板方法 findClass，调用 defineClass
- 自定义类加载器加载自**加密**的 class，防止反编译和篡改



#### 问题2 什么时候需要类加载器加载一个类

- Spring 动态代理 class 生成一个新的 class，使用时 Spring 加载新的 class
- JRebel 热部署，手动加载类到内存中





## 4. 执行模式

解释器：bytecode interpreter

JIT：Just In-Time compiler

混合模式：混合使用解释器和热点代码编译

- 起始使用解释器
- 热点代码检测后编译：多次调用的方法，多次调用的循环

模式设置：

- -Xmixed：混合模式
- -Xint：解释模式，启动快执行慢
- -Xcomp：纯编译，执行快启动慢





## 5. 懒加载

lazyloading / lazyInitializing

JVM 规范中没有规定加载时间，但是规定了**初始化**时间

- new / getstatic / putstatic / invokestatic 指令，访问 final 变量除外
- java.lang.reflect 反射调用时
- 初始化子类时，父类先初始化
- 虚拟机启动时，被执行的主类初始化
- 动态语言支持 invoke.MethodHandler 解析结果为 getstatic / putstatic / invokestatic 方法句柄时初始化



## 面试题

#### 1. Java 类加载过程

类加载器加载 class 文件，在 JVM 中形成描述 class 结构如构造函数属性方法的元信息对象。JVM 把元信息对象加载到内存，再验证，解析，初始化，最终形成可直接使用的 Java 类。

5个过程

1. 加载

   获取类的二进制流，将静态存储结构转换成方法区的运行时数据结构，内存中生成 Class 对象

2. 验证

   确保 Class 文件的字节流中的信息不会危害到虚拟机：

   - Class 文件格式规范验证
   - 元数据语义分析，继承关系等语义问题
   - 字节码验证，类型转换等语法问题
   - 符号引用验证，确保正确解析

3. 准备

   静态变量分配内存并初始化为默认值

4. 解析

   从符号引用到直接引用

5. 初始化

   执行类中定义的 Java 初始化代码，成为可使用的 Java 类




#### 2. Java 加载 Class 文件原理机制

类（Class）只有被加载到 JVM 后才能运行。

类加载器（ClassLoader 和他的子类）本身也是一个类，其实质是把类文件从硬盘读取到内存中。

- 隐式加载：new 的方式创建对象
- 显式加载：class.forName() 

每个类或接口都对应一个 .class 文件，看成是一个个可以被动态加载的单元，因此当只有部分类被修改时，只需要重新编译变化的类即可， 而不需要重新编译所有文件，因此加快了编译速度。

说明类加载过程：问题1



#### 3. Java 内存分配

- 寄存器：无法控制
- 静态域：static 定义的静态成员
- 常量池：编译时确定保存在 .class 文件中的 final 常量值和符号引用
- 非 RAM 存储：硬盘
- 堆内存：new 创建的对象和数组，由 JVM 的自动垃圾回收器管理，存取慢
- 栈内存：基本类型的变量和堆内存中对象的引用变量地址，速度快可共享，大小和生存期确定缺乏灵活性



#### 4. Java 堆的结构，堆中永久代 PermGen space

堆在 JVM 启动的时候被创建，是运行时数据区，所有类的实例和数组都在堆上分配内存。对象所占的堆内存由自动垃圾收集器回收。

堆内存是由存活和死亡的对象组成的。存活的对象是应用可以访问的，不会被垃圾回收。死亡的对象是应用不可访问尚且还没有被垃圾收集器回收掉的对象。垃圾收集器把这些对象回收掉之前，他们会一直占据堆内存空间。



#### 5. GC 是什么，为什么有

GabageCollection：垃圾收集

Java 语言没有提供释放已分配内存的显示操作方法。GC 能够自动监测对象是否超过作用域，达到自动回收内存的目的。





#### 6. Java 的 GC 机制

程序员不需要显示的去释放对象的内存，由虚拟机自行执行。

在 JVM 中，有一个垃圾回收线程。它是低优先级的，在正常情况下是不会执行，只有在虚拟机空闲或者当前堆内存不足时，才会触发执行。

扫描没有被任何引用的对象，将它们添加到要回收的集合中，进行回收。



#### 7. GC 判断对象是否存货的方法

- 引用计数法

  给每个对象设置引用计数器，每有一个地方引用该对象时，计数器加一，引用失效时，计数器减一。当对象的引用计数器为零时，说明此对象没有被引用，也就是“死对象”，会被垃圾回收。但会出现对象A和对象B互相引用形成循环引用，无法回收，主流 GC 不采用该算法。

- 可达性算法

  从 GC Roots 对象向下搜索，如果某对象到 GC Roots 没有引用链相连时，说明此对象不可用，两次标记后进行回收。

  - 第一次标记条件，判断该对象是否覆盖 finalize() 方法，没有则进入 F-Queue 队列。

    finalize() 方法执行过慢会导致内存回收系统崩溃

  - 第二次标记，触发 Finalize() 线程执行释放内存

  GC Roots：栈中引用对象，方法区类静态属性引用的对象，方法区常量池引用的对象，本地方法栈 JNI 引用对象



#### 8. GC 优点，回收机制

- 编程的时候不需要考虑内存管理

- 对象不再有作用域，对象的引用有作用域
- 防止内存泄漏，有效使用内存

种类：分代复制垃圾回收，标记垃圾回收，增量垃圾回收



#### 9. GC 原理，可以立即收回内存吗，主动通知 JVM 进行 GC

当创建对象时，GC 就开始监控对象的地址、大小以及使用情况。通常，GC 采用有向图的方式记录和管理堆（heap）中所有的对象，判断是否可达。当 GC 确定对象为“不可达”时，回收这些内存空间。

可以立即回收内存。

手动执行 System.gc()，通知 GC 运行，但是 Java 语言规范不保证 GC 一定会执行。



#### 10. Java 存在内存泄漏吗

**内存泄露**指一个不再被程序使用的对象或变量一直被占据内存。

Java 中有垃圾回收机制，可以保证一对象不再被引用的时候，将自动被垃圾回收器从内存中清除掉。Java 使用有向图的方式进行垃圾回收管理，可以消除引用循环的问题。

内存泄露的情况：无用且无法回收，长生命周期对象持有短生命周期对象的引用

- 堆栈存储了10个元素，全部弹出后堆栈为空无用，但是堆栈无法被回收。当push进新的引用时完成自愈
- HashSet 修改参与计算的哈希值字段，导致无法检索和删除

检查内存泄露：程序各分支情况完整执行到程序结束，某对象未被使用过则属于内存泄露



#### 11. 深拷贝和浅拷贝

深拷贝：为对象中存在的动态成员或指针重新开辟内存空间。

浅拷贝：为对象中的数据成员进行简单赋值，如果存在动态成员或者指针就会报错。



#### 12. System.gc() 和 Runtime.gc()

提示 JVM 要进行垃圾回收，立即开始还是延迟执行取决于 JVM 。



#### 13. finalize() 调用时间，析构函数 finalization 目的

GC 在决定回收对象时，调用该对象的 finalize() 方法。但是如果内存充足，可能永远不会 GC，使用 finalize() 方法对 Java 程序用处不大。

finalize() 方法主要用于回收特殊渠道申请的内存，如 JNI ( Java Native Interface ) 调用的非 Java 程序占用的内存。



#### 14. 对象的引用被置为 null，GC是否会立即释放内存

不会，在下一个垃圾回收周期才会被标记可被回收。



#### 15. 分布式垃圾回收 DGC 如何工作

RMI ( Remote Method Invocation ) ：分布式对象应用模型，使一个 JVM 中的对象，调用另一个 JVM 中的对象方法并获取调用结果。

RMI 使用 DGC 来做自动垃圾回收，通过引用计数算法来给远程对象提供自动内存管理。



#### 16. GC 收集器（G1 和 CMS 对比）

**串行 Serial 收集器**：复制算法，使用一条线程进行 GC ，工作时其他线程停止。对大多数 100M 左右的内存的小应用足够了。

**吞吐量 Throughput 收集器**：使用并行版本的新生代垃圾收集器，用于中等规模和大规模数据的应用程序。

**ParNew 收集器**：复制算法，多线程版 serial 收集器，控制时间。

**Parallel Scavenge 收集器**：复制算法，多线程收集器，控制吞吐量。

**Serial Old 收集器**：标记 - 整理法，老年代 GC。

**Parallel Old 收集器**：标记 - 整理法，优化吞吐量的老年代 GC。

**Current Mark Sweep CMS 收集器**：复制 + 标记清除法，老年代 GC。



G1 对比 CMS：

- G1 通过内存分区避免碎片，压缩空间
- G1 分区中 Eden/Survivor/Old 区不固定，内存使用效率更灵活
- G1 预设停顿时间控制垃圾收集时间，避免雪崩
- G1 回收内存后合并空闲内存，CMS 在 STW ( Stop The World ) 时合并内存
- G1 在年轻代 GC 中使用，CMS 在老年代 GC 中使用
- 吞吐量 G1 好，对内存块大小要求高。核心是整理，碎片空间小
- 响应快 CMS 好，对 cpu 资源要求高。核心是清除，会有很多内存碎片



#### 17. 内存分配及回收策略，Minor GC 和 Major GC

GC 分代优化性能，HotSpot JVM 把年轻代分为三部分：1个 Eden 区和 2个 Survivor 区（from和to），默认比例为 8 : 1。

对象经过第一次 Minor GC 后，仍然存活将会被移到 Survivor 区。在Survivor区中每熬过一次Minor GC，年龄就会增加1岁，当年龄增加到一定程度时，会被移动到年老代中。

- 对象优先在 Eden 区分配
- 大对象直接进老年代
- 长生命周期对象直接进老年代

Eden 区没有足够的空间进行分配时，虚拟机会执行一次 Minor GC。Minor GC 通常发生在年轻代的 Eden 区，在这个区的对象生存期短，发生 GC 的频率较高，回收速度比较快；

Full GC / Major GC 发生在老年代，一般触发老年代 GC  的时候不会触发 Minor GC。通过配置，可以在 Full GC 之前进行一次Minor GC ，可以加快老年代的回收速度。



#### 18. PermGen 中会有 GC 吗

永久代中没有 GC，如果满了或超临界值，触发 Full GC。

Java 8 中移除了永久代，新加了元数据区即 native 内存区。



#### 19. GC 方法

**标记 - 清除**：最基础的方法，标记要被回收的对象，然后统一回收。效率低，产生大量的不连续内存碎片。

**复制算法**：解决效率问题，将内存按容量划分为相等的两部分，每次只使用其中的一块，当一块内存用完时，将还存活的对象复制到第二块内存上，一次性清除第一块内存，再将第二块上的对象复制到第一块。内存的代价高，浪费一半的内存。

- **改进1**，**内存区域**不再是按照 1:1 去**划分**，而是划分为 8:1:1 三部分，较大那份内存为 Eden 区，其余是两块较小的内存区叫Survior 区。优先使用 Eden 区，若 Eden 区满，将对象复制到第二块内存区上，然后清除 Eden  区，如果此时存活的对象太多，以至于 Survivor 不够时，会将这些对象通过分配担保机制复制到老年代中。
- **改进2**，**标记 - 整理**：解决标记 - 清除产生大量内存碎片的问题，当对象存活率较高时，也解决复制算法的效率问题。不同之处在于清除对象的时候先将可回收对象移动到一端，然后清除掉端边界以外的对象，这样就不会产生内存碎片了。

**分代收集**：大多采用这种方式，根据对象的生存周期，将堆分为新生代和老年代。在新生代中，对象生存期短，每次回收会有大量对象死去，采用复制算法。老年代中的对象存活率较高，没有额外的空间进行分配担保。



#### 20. 类加载器及其种类

类加载器通过类的权限定名获取该类的二进制字节流。

主要有四种类加载器：

- 启动类加载器（Bootstrap ClassLoader）：加载 Java 核心类库，无法被 Java 程序直接引用
- 扩展类加载器（Extensions ClassLoader）：加载扩展库
- 系统类加载器（System ClassLoader）：根据 classpath 加载类，通过 ClassLoader.getSystemClassLoader() 来获取
- 自定义加载器：继承 java.lang.ClassLoader 类



#### 21. JVM 内存模型，分区及作用

线程共有：方法区，堆

线程私有：栈，本地方法栈，程序计数器

- 方法区：虚拟机加载类信息，常量和静态变量。也称永久代，很少发生 GC，主要卸载常量池和类信息。

- 堆：最大的一块，存放对象和数组实例

- 虚拟机栈：方法执行时内存模型，生命周期与线程一致，存放局部变量表，操作数栈，动态链接，方法出口
  - 栈溢出：方法创建了很大的对象Array，List；循环调用；引用了较大的全局变量
- 本地方法栈：服务于 native 方法
- 程序计数器：最小的一块，执行的行号指示器，选取下一行待执行的字节码指令



#### 22. JVM 对象创建流程

- JVM 接收 new 指令，检查参数能否在常量池定位到其引用，检查类是否加载解析初始化

- 如果没有加载，进行加载

- JVM 为对象实例申请内存空间

  - 内存规整：使用内存放一边，空闲内存放另一边，中间为指针分界器，向空闲内存移动对象大小的距离，叫“**指针碰撞**”
  - 内存不规整：使用的内存和空闲内存交错，维护一张记录可用内存的表，找一块空间分配内存并更新表

- 分配到线程共享的堆上，JVM 用 0 值初始化内存空间

  分配到 TLAB ( Thread local allocation buffer ) 线程本地分配缓存区，只占 Eden 区一小部分，直接分配到线程独有的栈上

- 信息设置，属于哪个类的对象，哈希值，GC 年代

- JVM 工作完成，交由 Java 程序，执行 init 初始化方法



#### 23. OOM 内存溢出的情况

- 堆溢出：java.lang.OutOfMemoryError：Java heap space
- 栈溢出：java.lang.StackOverflowError
- 永久代溢出：java.lang.OutOfMemoryError：PermGen space



#### 24. 常用的 JVM 参数

数据区：

- Xms：初始堆大小
- Xmx：最大堆大小
- Xss：每个线程的栈大小
- XX:NewSize=n：年轻代大小
- XX:NewRatio=n：年轻代与老年代比值
- XX:MaxPermSize=n：永久代大小



GC：

- XX:+UseSerialGC：设置串行 GC
- XX:+UseParallelGC：设置并行 GC
- XX:+UseParallelOldGC：设置并行老年代 GC
- XX:+UseConcMarkSweepGC：设置 CMS 并发 GC



GC 日志：

- XX:+PrintGC：打印简要信息
- XX:+PrintGCDetails：打印详细信息
- XX:+PrintGCTimeStamps：输出时戳



#### 25. 对象进入老年代的情况

- 大对象直接进老年代
- 长期存活的对象，在 Survivor 区中经历多次 Minor GC 后被晋升到老年代



#### 26. 内存溢出和内存泄露

内存溢出 OOM：申请内存时内存不足

内存泄露 Memory Leak：无法释放已申请的内存空间



#### 27. JVM 常用工具

- jps：显示本地的 Java 进程
- jinfo：运行环境参数
- jstat：监视 JVM 运行状态
- jstack：查看线程运行情况和运行状态
- jmap：物理内存占用情况
- jhat：查看堆信息



#### 28. Minor GC / Major GC / Full GC

- Minor GC：年轻代 GC
- Major GC：老年代 GC
- Full GC：年轻代，老年代，永久代 / 元空间的全局GC
  - 调用 System.gc()，建议系统执行 Full GC
  - 老年代空间不足
  - 方法区空间不足
  - Minor GC 后老年代内存不足
  - Eden 区和 Survivor From 向 Survivor To 区复制时，内存不足，转存到老年代内存也不足



#### 29. 强引用，软引用，弱引用，虚引用和 GC

- 强引用：必不可少的引用，宁愿抛出内存溢出异常也不会回收
- 软引用：非必须对象的引用，内存溢出才会回收
- 弱引用：非必须的对象，无论内存是否充足，下一次 GC 都会回收
- 虚引用：生存时间无影响，回收时通知，与引用队列关联，用于跟踪 GC 活动



#### 30. Marshalling 和 Demarshalling，Serialization 和 Deserialization

当应用程序把内存对象跨网络传递到另一台主机或持久化的时候，就必须把对象在内存里面的表示转化成合适的格式

转换的过程就叫做 Marshalling，反之 Demarshalling



Java 提供了一种叫做对象序列化的机制，把对象表示成一连串的字节，里面包含了对象的数据，对象的类型信息，对象内部的数据的类型信息等

序列化可以看成是为了把对象存储在磁盘上或者是从磁盘上读出来并重建对象而把对象扁平化的一种方式，反序列化是把对象从扁平状态转化成活动对象的相反的步骤





























