## 面试题

#### 1. 什么是 SpringBoot

随着功能增加 Spring 变得复杂，开启新的 Spring 项目需要从头做的工作很多。

Spring Boot 建立在现有 spring 框架之上，避免 Spring 必须的样板代码和配置，以最少的工作量，更健壮地使用 Spring 功能



#### 2. SpringBoot 优点

- 减少开发和测试时间

- 使用 Java Config 避免使用 XML

- 避免大量的 Maven 导入和各种版本冲突

- 提供默认值，快速开始开发

- 不需要单独的 Web 服务器，不再需要启动 Tomcat，Glassfish

- 更少的配置，没有 web.xml 文件。只需添加用 @Configuration 注释的类，然后用 @Bean 注释的方法，Spring 将自动加载对象并像以前一样对其进行管理。可以将 @Autowired 添加到 bean 方法中，以使 Spring 自动装入需要的依赖关系

- 配置环境：- Dspring.profiles.active = {enviornment}。

  在加载主应用程序属性文件后，Spring 将在 application{environment} .properties 中加载后续的应用程序属性文件



#### 3. JavaConfig

提供配置 Spring IoC 容器的纯 Java 方法，有助于避免使用 XML 配置。

优点：

- 面向对象的配置

- 减少或消除 XML 配置。基于依赖注入原则的外化配置的好处已被证明，JavaConfig 为开发人员提供了一种纯 Java 方法来配 置与 XML 配置概念相似的 Spring 容器。

- 类型安全和重构友好

  Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找



#### 4. 如何加载 SpringBoot 上的更改，而无需重启服务器

DevTools 模块，生产环境中被禁用

配置 org.springframework.boot：spring-boot-devtools = true



#### 5. 监视器 Actuator

监视器模块公开了一组可直接作为 HTTP URL 访问的 REST 端点来检查正在运行应用程序的状态

一些外部程序使用这些服务向运维人员发送警报消息



#### 6. 如何禁用 Actuator 的端点安全性

默认所有敏感的 HTTP 端点都只有具有 ACTUATOR 角色的用户才能访问，使用HttpServletRequest.isUserInRole 方法实施

禁用安全性：management.security.enabled = false

只有在执行机构端点，在防火墙后访问时，才建议禁用安全性



#### 7. CSRF 攻击

跨站请求伪造，这是一种攻击，迫使最终用户在当前通过身份验证的 Web 应用程序上执行不需要的操作



#### 8. WebSockets

一种计算机通信协议，通过单个 TCP 连接，提供全双工通信信道

- 双向：客户端和服务端都能发送消息
- 全双工：客户端和服务器相互独立
- 单个 TCP 连接：初始连接使用 HTTP，然后将此连接升级到基于 Socket 的连接，此后这个单一连接用于所有未来的通信
- 轻量级：与 HTTP 相比，WebSocket 消息数据交换更轻



#### 9. 核心配置文件

application 和 bootstrap 公用一个环境，不能被本地相同配置覆盖

application：用于自动化配置

bootstrap：父上下文，优先于 application 加载

- 使用 Spring Cloud Config 配置中心时，需要在 bootstrap 配置文件中添加连接到配置中心的配置属性加载配置信息
- 固定的不能被覆盖的属性
- 加密 / 解密的场景



#### 10. 核心注解

@SpringBootApplication 包含注解：

- @Configuration：实现配置文件

- @EnableAutoConfiguration：打开自动配置功能，也可以关闭

  如 @SpringBootApplication(exclude =  { DataSourceAutoConfiguration.class })

- @ComponentScan：组件扫描



#### 11. 开启 SpringBoot 项目特性的方式

- 继承 spring-boot-starter-parent 项目
- 导入 spring-boot-dependencies 项目依赖



#### 12. SpringBoot 需要独立容器运行吗

不需要，内置了 Tomcat / Jetty 容器



#### 13. 运行 SpringBoot 的方式

- 打包进容器运行
- Maven 插件运行
- main 方法运行



#### 14. 自动配置原理

- @EnableAutoConfiguration
- @Configuration
- @ConditionalOnClass

自动配置的核心注解，首先是一个配置文件，其次根据类路径下是否有这个类去自动配置



#### 15. Starters

启动器，包含可以集成到应用中的依赖包



#### 16. 在 SpringBoot 启动时运行一些代码

实现接口 ApplicationRunner 或 CommandLineRunner，都只有一个 run 方法



#### 17. 读取配置的方式

绑定配置变量

- @PropertySource
- @Value
- @Environment
- @ConfigurationProperties



#### 18. 日志框架

- Java Util Logging
- Log4j2
- Lockback

使用 Starters 启动器，使用 Logback 作为默认日志框架



#### 19. 热部署

- Spring Loaded
- Spring-boot-devtools



#### 20. 配置加载顺序

不会覆盖，谁先指定就按谁的来

1. 开发者工具 `Devtools` 全局配置参数
2. 单元测试上的 `@TestPropertySource` 注解指定的参数
3. 单元测试上的 `@SpringBootTest` 注解指定的参数
4. 命令行指定的参数，如 `java -jar springboot.jar --name="Java技术栈"`
5. 命令行中的 `SPRING_APPLICATION_JSONJSON` 指定参数, 如 `java -Dspring.application.json='{"name":"Java技术栈"}' -jar springboot.jar`
6. `ServletConfig` 初始化参数
7. `ServletContext` 初始化参数
8. JNDI参数，如 `java:comp/env/spring.application.json`
9. Java系统参数，来源：`System.getProperties()`
10. 操作系统环境变量参数
11. `RandomValuePropertySource` 随机数，仅匹配：`ramdom.*`
12. JAR包外面的配置文件参数`application-{profile}.properties（YAML）`
13. JAR包里面的配置文件参数`application-{profile}.properties（YAML）`
14. JAR包外面的配置文件参数`application.properties（YAML）`
15. JAR包里面的配置文件参数`application.properties（YAML）`
16. `@Configuration`配置文件上 `@PropertySource` 注解加载的参数
17. 默认参数，通过 `SpringApplication.setDefaultProperties` 指定 



#### 21. 提供不同的环境

- 多套配置文件，并在 spring.profiles.active 中激活
- Java 代码中用 @Profile 注解指定
- main 方法启动参数指定
- 插件启动参数指定
- jar 包运行参数指定

















