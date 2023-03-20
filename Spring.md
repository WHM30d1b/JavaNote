## 面试题

### Spring

#### 1. 什么是Spring

简化 Java 企业级应用开发的开源开发框架。



#### 2. Spring 优势

优势：

- 轻量：基本的版本约 2MB
- 控制反转：实现松散耦合，对象们给出它们的依赖，而不是创建或查找依赖
- 面向切面的编程(AOP)：把应用业务逻辑和系统服务分开
- 容器：包含并管理应用中对象的生命周期和配置
- MVC 框架：WEB 框架
- 事务管理：提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务
- 异常处理 ：提供 API 把具体技术比如 JDBC 抛出的相关的异常转化为 unchecked 异常



#### 3. 组成模块

核心模块：

- Core module：控制反转和依赖注入

- Bean module：BeanFactory，工厂模式的实现，把应用的配置和依赖从代码中分离。

  XmlBeanFactory 根据 xml 中定义加载 beans

- Context module：核心容器，Spring 框架建立在其之上，使 Spring 成为一个容器

  建立在 Core 和 Bean 模块的基础之上，提供框架式的对象访问方式，**访问**，**定义**，**配置**对象的媒介

- Context-support module：整合第三方库到Spring应用程序上下文

- Expression Language module：强大的表达式语言去支持运行时查询和操作对象图



AOP 模块：

- Spring-aop module：符合 AOP 要求的面向切面的编程实现
- Spring-aspects module：与 AspectJ 的集成功能，AspectJ是一个功能强大且成熟的AOP框架
- Spring-instrument module：AOP 的支援模块，提供了类植入支持和类加载器的实现，可以在特定的应用服务器中使用。主要作用是在 JVM 启用时， 生成一个代理类，程序员通过代理类在运行时修改类的字节，从而改变一个类的功能， 实现 AOP 的功能。



数据访问和集成：

- JDBC module：JDBC的抽象层，简化 JDBC 编程
- ORM module：ORM 框架支持模块
- OXM module：Object-to-XML-Mapping，将 java 对象映射成 XML 数据，或将 XML 数据映射成 java 对象
- Java Messaging Service(JMS) module：Java 消息传递服务，包含用于生产和使用消息的功能

- Transaction module：事务模块



Web：

- Web module：基本的Web开发集成功能
- Spring-websocket module：双工异步通讯协议，实现 Socket 通信和 web 端的推送功能
- spring-webmvc module / Web-Servlet module：Spring MVC 和 REST Web Services 实现
- Portlet module：web 模块功能的聚合，提供了Portlet环境下的MVC实现
- Spring-webflux module：非堵塞函数式 Reactive Web 框架，用来建立异步的非阻塞事件驱动的服务，扩展性非常好
  

Messaging：

- Spring messaging module：消息传递的体系结构和协议



Test：

- Spring-test module：使用 JUnit 或 TestNG 对 Spring 组件进行单元测试和集成测试



#### 4. 什么是 IOC 容器

IOC：Inversion of Control 控制反转

Spring IOC 依赖注入负责创建对象，管理对象，装配对象，配置对象，并管理这些对象的整个生命周期。

优点：

- 降低代码量
- 容易测试
- 最小的侵入促使松散耦合
- 支持饿汉式初始化和懒加载



#### 5. ApplicationContext 的实现

- FileSystemXmlApplicationContext：从一个 XML 文件中加载 beans 的定义，**XML Bean 配置文件的全路径名**必须提供给它的构造函数
- ClassPathXmlApplicationContext：从一个 XML 文件中加载 beans 的定义，需要正确设置 **classpath** 因为这个容器将在classpath 里找 bean 配置
- WebXmlApplicationContext：加载一个 **XML 文件**，此文件定义了一个 WEB 应用的所有 bean



#### 6. Spring 应用像什么

- 一个定义了一些功能的接口
- 类的实现包括属性，Setter，Getter 方法和函数等
- Spring AOP
- Spring 的 XML 配置文件
- 使用以上功能的客户端程序



#### 7. Bean 工厂和 Application Context 的区别

BeanFactory 是含有 bean 集合的工厂类，包含各种 bean 的定义，在接收到客户端请求时将对应的 bean 实例化，同时生成协作类之间的关系。BeanFactory 包含 bean 生命周期的控制，调用客户端的初始化方法 （initialization methods）和销毁方法（destruction methods）。

表面上看，application context 如同 bean factory 一样，但 application context 在此基础上还提供了其他的功能。

1. 提供了支持国际化的文本消息 
2. 统一的资源文件读取方式
3. 向注册的监听器中发布 bean 的事件



### 依赖注入

#### 8. 什么是依赖注入

IOC：

- 不用创建对象，只需要描述如何创建
- 不在代码里直接组装组件和服务，但在配置文件里描述哪些组件需要哪些服务，之后一个 IOC 容器 负责组装。



#### 9. 依赖注入的方式

- 构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现，该类有一系列参数，每个参数代表一个对其他类的依赖。实现强制依赖。
- Setter 方法注入：Setter 方法注入是容器通过调用无参构造器或无参 static 工厂方法实例化 bean 之后，调用该 bean 的setter 方法实现依赖注入。实现可选依赖。
- 接口注入



### Beans

#### 10. 什么是 Spring Beans

形成 Spring 应用主干的 java 对象，被 Spring IOC 容器初始化，装配和管理。这些 beans 通过容器中配置的元数据创建，如以 XML 文件形式定义。

Spring 框架定义的 beans 都是单件。在 bean tag 中有个属性 ”singleton” ， 如果被赋为 TRUE 就是单件 bean，否则就是一个 prototype bean，默认是 TRUE。

一个 Spring Bean 包含容器必知的所有配置元数据，包括**如何创建一个 bean**，**生命周期**详情及**依赖**。



#### 11. 给 Spring 容器配置元数据的方式

- XML 配置
- 注解配置
- java 配置



#### 12. 定义类的作用域

通过 bean 中的 scope 属性来定义。

- **singleton**：默认值，在需要的时候每次生产一个新的 bean 实例，scope 属性被指定为 prototype。

- **prototype**：bean 每次使用的时候必须返回同一个实例，scope 属性必须设为 singleton。

- **request**：每次 http 请求都会创建一个bean。

  该作用域仅在基于 web 的 Spring ApplicationContext 情形下有效。

- **session**：在一个 HTTP Session 中，一个 bean 定义对应一个实例。

  该作用域 仅在基于 web 的 Spring ApplicationContext 情形下有效。

- **global-session**：在一个全局的 HTTP Session 中，一个 bean 定义对应一个实例。

  该作用域仅在基于 web 的 Spring ApplicationContext 情形下有效。



#### 13. 单例 Bean 线程安全吗？

不是线程安全的

Spring 框架并没有对单例 bean 进行任何多线程的封装处理。实际上，大部分的 Spring bean 并没有可变的状态，所以在某种程度上说 Spring 的单例 bean 是线程安全的。

如果有多种状态的话，需要自行保证线程安全。最浅显的解决办法就是将多态 bean 的作用域由“singleton”变更为“prototype”。



#### 14. Bean 的生命周期

Bean 的生命周期主要是创建bean的过程，一个bean的生命周期主要是4个步骤： 实例化，属性注入，初始化，销毁

对于一些复杂 Bean 的创建，spring 在 Bean 的生命周期中开放很多的接口，可以在加载 bean 的时候做一些改变

1. **实例化之前**：
   1. 实现了 BeanFactoryPostProcessor 接口的 Bean，在加载其他的 Bean 的时候，也会调用这个 Bean 的 postProcessBeanFactory 方法，可以在这个步骤读取 bean 的定义并修改赋值。
   2. 实现了 InstantiationAwareBeanPostProcessor 接口的 Bean，会在实例化之前调用 postProcessBeforeInstantiation方法
2. 对bean进行**实例化**
3. 对bean进行**属性注入**
4. 对bean进行**初始化**，在初始化中，包含了以下几个步骤：
   1. 实现了 BeanFactoryAware 接口，会先调用 setBeanFactory 方法
   2. 实现了 BeanNameAware 接口，会先调用 setBeanName 方法
   3. 实现了 BeanPostProcessor 接口，会先调用 postProcessBeforeInitialization 方法
   4. 实现了 InitializingBean 接口，会调用 afterPropertiesSet 方法
   5. 再进行aop后置处理，通过实现 BeanPostProcessor 接口，在 postProcessAfterInitialization 方法中进行动态代理
5. **销毁**
   1.  Bean 实现了 DisposableBean 接口，Spring 将调用其 destory() 接口方法



#### 15. 内部 Bean

一个 bean 仅被用作另一个 bean 的属性时，声明为一个内部 bean

为定义 inner bean，在 XML 文件配置元数据中，property 标签或 constructor-arg 标签内使用 bean 子标签

内部 bean 通常是匿名的，Scope 一般是 prototype



#### 16. 注入一个 java 集合

- list 类型用于注入一列值，允许有相同的值
- set 类型用于注入一组值，不允许有相同的值
- map 类型用于注入一组键值对，键和值都可以为任意类型
- props 类型用于注入一组键值对，键和值都只能为 String 类型。 



#### 17. 装配和自动装配

装配：Spring 容器中把 bean 组装到一起，前提是容器需要知道 bean 的依赖关系和如何通过依赖注入装配

自动装配：Spring 容器能通过 Bean 工厂自动处理 bean 之间的协作，自动装配相互合作的 bean



自动装配的方式：

- no：默认的方式，不进行自动装配，通过显式设置 ref 属性来进行装配
- byName：通过参数名自动装配，容器试图匹配、装配和该 bean 的属性具有相同名字的 bean
- byType：通过参数类型自动装配，容器试图匹配、装配和该 bean 的属性具有相同类型的 bean。如果有多个 bean 符合条件，则抛出错误
- constructor：类似于 byType， 要提供给构造器参数，如果没有确定的带参数的构造器参数类型会抛出异常
- autodetect：首先尝试使用 constructor 来自动装配，如果无法工作，则使用 byType 方式



自动装配的局限性：

- 重写：仍需用 constructor-arg 和 property 来定义依赖，要重写自动装配
- 基本数据类型：不能自动装配简单的属性，如基本数据类型，String 字符串和类
- 模糊：不如显式装配精确



### 注解

#### 18. 什么是注解

基于 Java 的配置，允许在少量的 Java 注解的帮助下，进行大部分 Spring 配置而非通过 XML 文件。

@Configuration 注解：用来标记类可以当做一个 bean 的定义，被 Spring IOC 容器使用

@Bean 注解：表示此方法将要返回一个对象，作为一个 bean 注册进 Spring 应用上下文



基于注解的容器配置：通过在相应的类，方法或属性上使用注解的方式，直接在组件类中进行配置



#### 19. 开启注解装配

注解装配默认不开启，使用需要在 Spring 配置文件中配置  context : annotation-config 标签



#### 20. @Required

bean 的属性必须在配置的时候设置，通过一个 bean 定义的显式的属性值或通过自动装配

若 @Required 注解的 bean 属性未被设置，容器将抛出 BeanInitializationException



#### 21. @Autowired

提供更细粒度的控制，包括在何处以及如何完成自动装配。

用法和 @Required 一样，修饰 setter 方法、构造器、属性



#### 22. @Qualifier

有多个相同类型的 bean 却只需要自动装配一个时，@Qualifier 注解和 @Autowire 注解联用指定确切需要装配的 bean



### 数据访问

#### 23. JDBC

使用 SpringJDBC 框架，开发者只需写 statements 和 queries 从数据存取数据，也可以用 JDBC 模板类 JdbcTemplate

JdbcTemplate 类提供了很多便利的方法：

- 把数据库数据转变成基本数据类型或对象
- 执行可调用的数据库操作语句
- 提供自定义的数据错误处理



#### 24. 支持数据访问对象 DAO

旨在简化数据访问技术，方便切换持久层



#### 25. Spring 支持的 ORM

- Hibernate
- MyBatis
- JPA (Java Persistence API)
- TopLink
- JDO (Java Data Objects)
- OJB 



#### 26. 事务

编程式事务管理：通过编程的方式管理事务，灵活性好但是难维护

声明式事务管理：将业务代码和事务管理分离，用注解和 XML 配置来管理事务

声明式事务管理对应用代码的影响最小，更符合一个无侵入的轻量级容器的思想。



优点：

- 为不同事务 API 提供相同的编程模式
- API 简单
- 支持声明式事务管理
- 能够集成多种数据访问抽象层



### 面向切面 AOP

#### 27. 什么是 AOP

Aspect Oriented Programming：面向切面编程。一种编程技术，允许程序模块化横向切割关注点，如日志和事务管理



#### 28. 什么是 Aspect 切面

AOP 将影响多个类的公共行为封装到一个可重用的模块中，并命名为`Aspect`切面。日志模块可以被称作日志的 AOP 切面。

根据需求的不同，一个应用程序可以有若干切面，通过带有@Aspect 注解的类实现。

使用场景：

- 日志记录
- 性能统计
- 安全控制
- 事务处理
- 异常处理



#### 29. 什么是连接点 / 切入点

连接点：代表应用程序的某个位置，在这个位置可以插入一个 AOP 切面，实际上是应用程序执行 Spring AOP 的位置

切入点：一个或一组连接点，通知将在这些位置执行，可以通过表达式或匹配的方式指明切入点



#### 30. 通知

方法执行前或执行后要做的动作，实际上是程序执行时要通过 Spring AOP 框架触发的代码段。

Spring 切面可以应用五种类型的通知：

- before：前置通知，在一个方法执行前被调用
- after：在方法执行之后调用的通知，无论方法执行是否成功
- after-returning：仅当方法成功完成后执行的通知
- after-throwing：在方法抛出异常退出时执行的通知
- around：在方法执行之前和之后调用的通知



#### 31. 什么是目标对象

被切面通知的对象



#### 32. 什么是代理对象

通知目标对象后创建的对象叫代理对象

自动代理种类：

- BeanNameAutoProxyCreator
- DefaultAdvisorAutoProxyCreator
- Metadata autoproxying



#### 33. 什么是织入

将切面和到其他应用类型或对象连接或创建一个被通知对象的过程。织入可以在编译时，加载时，或运行时完成



### Spring MVC

#### 34. 什么是 Spring MVC / 优点 / 原理

SpringMVC 是 Spring 的一个模块，基于 MVC 的框架，无需中间层即可整合。

构建全功能 Web 应用的 MVC 框架。

Spring 的 MVC 框架用控制反转把业务对象和控制逻辑清晰地隔离，允许以声明的方式把请求参数和业务对象绑定



优点：

- 基于组件技术，全部的应用对象，无论控制器和视图，还是业务对象之类的都是 java 组件，和 Spring 提供的其他基础结构紧密集成
- 底层依赖 Servlet，开发者不依赖于 Servlet API
- 使用各种视图技术，不局限于 JSP
- 支持各种请求资源的映射策略
- 易于扩展



工作原理：

- 用户发送请求至 **DispatcherServlet 前端控制器**
- DispatcherServlet 收到请求，调用 **HandlerMapping 处理器映射器**
- 处理器映射器根据 xml 配置、注解找到具体的 **Controller 处理器**，生成处理器对象及处理器拦截器一并返回给DispatcherServlet
- DispatcherServlet 调用 **HandlerAdapter 处理器适配器**
- HandlerAdapter 经过适配调用具体的 Controller
- Controller 执行完成返回 **ModelAndView**
- HandlerAdapter 将 ModelAndView 返回给 DispatcherServlet
- DispatcherServlet 将 ModelAndView 传给 **ViewReslover 视图解析器**
- ViewReslover 解析后返回具体 **View**
- DispatcherServlet 根据 View 进行渲染视图
- DispatcherServlet 响应用户 



#### 35. Controller 是不是单例模式，如何保证线程安全问题

Controller是单例的，是非线程安全的。并发请求调用 Controller 生成的是同一对象，线程共享 Controller 的实例对象

解决方案：

- 避免定义全局变量，局部变量不存在线程安全问题
- 使用 ThreadLocal 来进行线程隔离
  - 本地线程变量，ThreadLocal 中填充当前线程的变量，对其他线程而言封闭且隔离
  - 把对象封闭在一个线程里，即使这个对象不是线程安全的，也不会出现并发安全问题

#### 36. WebApplicationContext

继承 ApplicationContext 并增加了一些 WEB 应用必备的特有功能，能处理主题，并找到被关联的 servlet



#### 37. 控制器

控制器解析用户输入并将其转换为一个由视图呈现给用户的模型。

@Controller 注解表明该类扮演控制器的角色



#### 38. 怎样把 ModelMap 里面的数据放入 Session 里面？

在类上面加上 @SessionAttributes 注解，里面包含的字符串就是要放入 session 里面 的 key



### 其他

#### 39. Spring 事件

Spring 提供了以下 5 中标准的事件：

1. 上下文更新事件（ContextRefreshedEvent）：ApplicationContext 被初始化或者更新时发布，也在调用 ConfigurableApplicationContext 接口中的 refresh()方法时被触发
2. 上下文开始事件（ContextStartedEvent）：容器调用 ConfigurableApplicationContext 的 Start() 方法开始容器时触发
3. 上下文停止事件（ContextStoppedEvent）：容器调用 ConfigurableApplicationContext 的 Stop()方法停止容器时触发
4. 上下文关闭事件（ContextClosedEvent）：ApplicationContext 被关闭时触发该事件，容器被关闭时，其管理的所有单例 Bean 都被销毁
5. 请求处理事件（RequestHandledEvent）：Web 应用中，http 请求（request）结束触发该事件



#### 40. Spring 中的设计模式

- 代理模式：AOP 和 remoting
- 单例模式：bean 默认为单例模式
- 模板方法：解决代码重复的问题，如 RestTemplate，JmsTemplate，JpaTemplate
- 前端控制器：DispatcherServlet 对请求进行分发
- 视图帮助 (View Helper)：Spring 提供了一系列的 JSP 标签，高效宏来辅助将分散的代码整合在视图里
- 依赖注入：贯穿于 BeanFactory / ApplicationContext 接口的核心理念
- 工厂模式：BeanFactory 创建对象的实例



#### 41. SpringMvc 怎么和 AJAX 相互调用？

通过 Jackson 框架把 Java 里面的对象直接转化成 Js 可以识别的 Json 对象。

具体步骤：

- 加入 Jackson.jar
- 配置文件中配置 json 的映射
- 在接受 Ajax 方法里面可以直接返回 Object，List 等，方法前面加上 @ResponseBody 注解



#### 42. 配置拦截器

```xml
<!-- 配置 SpringMvc 的拦截器 -->
<mvc:interceptors>
    <!-- 配置一个拦截器的 Bean ,默认对所有请求都拦截 -->
    <bean id="myInterceptor" class="com.et.action.MyHandlerInterceptor"></bean>
    
    <!-- 针对部分请求拦截 -->
    <mvc:interceptor>
        <mvc:mapping path="/modelMap.do" />
        <bean class="com.et.action.MyHandlerInterceptorAdapter" />
    </mvc:interceptor>
    
</mvc:interceptors>
```



#### 43. 构造方法注入 和 设值注入

- 设值注入方法**支持大部分的依赖注入**，如果仅需要注入 int、string 和 long 型的变量，不要用设值的方法注入。对于基本类型，如果没有注入，可以设置默认值。

  构造方法注入不支持大部分的依赖注入，在调用构造方法中必须传入正确的构造参数，否则报错

- 构造方法在对象被创建时调用，设值注入**不会覆盖构造方法的值**

- 设值注入**不能保证依赖被注入**，对象的依赖关系可能不完整；构造器注入不允许生成依赖关系不完整的对象

- 设值注入时，如果对象 A 和 B 互相依赖，创建 A 时抛出 ObjectCurrentlyInCreationException 异常，**解决循环依赖问题**



#### 44. FileSystemResource 和 ClassPathResource 的区别

- FileSystemResource 需要给出 spring-config.xml 文件在项目中的相对路径或者绝对路径

- ClassPathResource 中 Spring 会在 ClassPath 中自动搜寻配置文件，所以要把 ClassPathResource 文件放在 ClassPath 下。如果将 spring-config.xml 保存在了 src 文件夹下的话，只需给出配置文件的名称即可，因为 src 文件夹是默认。

ClassPathResource 在环境变量中读取配置文件，FileSystemResource 在配置文件中读取配置文件



































