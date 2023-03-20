## 面试题

#### 1. String

- 不是 Java 基本类型
- final 类型不可变，不能被继承
- 可以自定义 java.lang.String 类且编译成功，但类加载机制不允许使用



#### 2. 怎么比较两个字符串值一样，对象一样？

值相同 equals

对象相同 ==



#### 3. switch 中可以使用 String 判断吗

JDK7+ 可以



#### 4. String str = new String("abc") 创建了几个对象

创建了两个对象：

- "abc"本身创建在常量池
- new 又创建在堆中



#### 5. String，StringBuffer，StringBuilder

- String 不可变；StringBuffer，StringBuilder 可变
- StringBuffer 线程安全，速度较慢；StringBuilder 非线程安全，速度较快



#### 6. String.trim()

去掉字符串首尾的空白字符



#### 7. String 和 byte[] 转换

String > byte[]：String 类的 getBytes 方法

byte[] > String：new String( byte[] ) 构造方法









