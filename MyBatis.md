## 面试题

#### 1. #{} 和 ${}

- **#{}**：预编译处理，将 sql 中的 #{} 替换为 ? 号，调用 PreparedStatement 的 set 方法来赋值，防止 SQL 注入

- **${}**：字符串替换，把${}替换成变量的值



#### 2. XML 映射文件会对应 Dao / Mapper 接口

接口的**工作原理**？

- XML 文件通过 namespace 指定接口的全限定名

- 每个 sql 标签会被解析为一个 MappedStatement 对象，id 值为接口的方法名
- 传递给 sql 的参数对应接口方法内的参数
- 接口没有实现类，接口名 + 方法名可以唯一指定 MappedStatement 对象
- JDK 动态代理为 Mapper 接口生成 proxy 对象，代理对象拦截接口方法，转而执行 MappedStatement 中的 sql



参数不同时能**重载**吗?

- 全限定名 + 方法名的查询策略，唯一指定对象，不能重载



#### 3. 分页

Mybatis 使用 **RowBounds 对象**进行分页，针对 ResultSet 结果集执行**内存分页**。

RowBounds 有 **offset** 和 **limit** 两个参数，表示从第几条开始，取多少条。

数据量大时慎用 RowBounds，会导致 OOM。



#### 4. 结果集封装为对象

建立列名和属性名的映射关系：

- 使用  <resultMap> 标签，逐一定义列名和对象属性名之间的映射关系
- 使用 sql 列的别名，将列别名书写为对象属性名

列名与属性名建立了映射关系后，Mybatis 通过反射创建对象，同时把反射给对象的属性逐一赋值并返回。



#### 5. XML 中常见的 sql 标签

- select | insert | update | delete
- trim | where | set | foreach | if | choose | when | otherwise | bind



#### 6. 插件

MyBatis 支持对以下接口的拦截：

- ParameterHandler
- ResultSetHandler
- StatementHandler
- Executor 

Mybatis 使用 JDK 的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这 4 种接口对象的方法时，就会进入拦截方法。

实现 Mybatis 的 Interceptor 接口并复写 intercept() 方法，给插件注解指定要拦截哪一个接口的哪些方法，还需要在配置文件中配置编写的插件。



#### 7. 一级缓存，二级缓存

MyBatis 的缓存分为一级缓存和二级缓存，一级缓存放在 session，默认开启，二级缓存放在命名空间，默认不开启，使用二级缓存属性类需要实现 Serializable 序列化接口，可用来保存对象的状态

- 一级缓存：基于 PerpetualCache 的 HashMap 本地缓存，存储作用域为 Session。

  当 Session flush 或 close 后，所有 Cache 被清空。

- 二级缓存：基于 PerpetualCache 的 HashMap 本地缓存，存储作用域为 Mapper (Namespace) 。

  可自定义存储源，如Ehcache。在 SQL 映射文件中添加 <cache/> 开启二级缓存。

- 缓存数据更新机制：当某作用域进行了 C/U/D 后，默认该作用域下所有 select 中的缓存将被 clear。



#### 8. 懒加载

是否启用懒加载：lazyLoadingEnabled = true|false

支持 association 关联对象（一对一）和 collection 关联集合对象（一对多）的懒加载

原理：用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，查询事先保存好的值进行调用。



#### 9. XML 标签解析

标签之间存在引用关系，但是可以不分顺序，**解析 - 标记 - 再解析**

- Mybatis 解析 A 标签，发现 A 标签引用了 B 标签但是尚未解析到

- 此时，Mybatis 将 A 标签标记为未解析状态，继续解析余下的标签
- 待所有标签解析完毕，Mybatis 重新解析被标记为未解析的标签
- 此时解析 A 标签时，B 标签已经存在，A 标签正常解析完成



#### 10. XML 文件和内部数据结构的映射关系

XML 标签配置信息封装到重量级对象 Configuration 内部，不同的标签被解析为不同的对象。

Mybatis 将所有 Xml 配置信息都封装到 All-In-One 重量级对象 Configuration 内部。在 Xml 映射文件中：

- parameterMap 标签会被解析为ParameterMap 对象，其每个子元素会被解析为 **ParameterMapping 对象**
- resultMap 标签会被解析为 ResultMap 对象，其每个子元素会 被解析为 **ResultMapping 对象**
- SQL 标签 select，insert，update，delete 均会被解析为 **MappedStatement 对象**
- SQL 标签内的 SQL语句会被解析为 **BoundSql 对象**



#### 11. 动态 SQL

动态 SQL：完成逻辑判断和动态拼接 sql 的功能。

9 种动态 SQL 标签：trim | where | set | foreach | if | choose | when | otherwise | bind

原理：使用 OGNL 从 sql 参数对象中计算表达式的值，根据表达式的值动态拼接 sql，以完成动态 sql 的功能。

- OGNL ( Object Graph Navigation Language )：表达式语言，用于数据访问，存取属性调用方法



#### 12. MyBatis 半自动 ORM 映射工具，与全自动的区别

Hibernate：全自动 ORM 映射工具，查询关联对象或关联集合对象时，可根据对象关系模型直接获取。

Mybatis：半自动 ORM 映射工具，查询关联对象或关联集合对象时，需要手动编写 sql。



#### 13. 接口绑定

接口映射：在 MyBatis 中定义接口，然后把接口里的方法和 SQL 语句绑定。

实现方式：

- XML 映射文件指定接口全路径名，并配置 SQL
- @Select注解 + SQL 语句



#### 14. 关联查询

**一对一**

两张表主外键关联

**嵌套结果**：

```xml
<select id="getClasses" resultMap="getClassesMap" parameterType="int">
    select * from classes c ,teacher t
    where c.tid=t.tid and c.tid=#{tid}
</select>

<!-- 自定义一个结果集类型，用外键关联两张表 -->
<resultMap type="one.to.one.Classes" id="getClassesMap">
    <id column="cid" property="cid"/>
    <result column="cname" property="cname"/>
    <association property="teacher" javaType="one.to.one.Teacher">
        <id column="tid" property="tid"></id>
        <result column="tname" property="tname"/>
    </association>
</resultMap>
```

嵌套查询：

```xml
<select id="getClasses2" resultMap="getClassesMap2">
    select * from classes c where c.cid = #{cid}
</select>
<resultMap type="one.to.one.Classes" id="getClassesMap2">
    <id column="cid" property="cid"/>
    <result column="cname" property="cname"/>
    <collection property="teacher" column="tid" select="getTeacherCollection">
    </collection>
</resultMap>
<select id="getTeacherCollection" resultType="one.to.one.Teacher">
    select tid tid,tname tname from teacher where tid=#{tid}
</select>
```



**多对一**

```xml
<select id="getClasses" resultMap="getClassesMap">
    select * from classes c,student s where s.cid=c.cid and c.cid=#{cid}
</select>
<resultMap type="one.to.many.Classes" id="getClassesMap">
    <id column="cid" property="cid"></id>
    <result column="cname" property="cname"/>
    <collection property="students" ofType="one.to.many.Student">
        <id column="sid" property="sid"/>
        <result column="sname" property="sname"/>
    </collection>
</resultMap>
```



**一对多**

```xml
<select id="getStudents" resultMap="getStudentMap">
    select * from classes c,student s where s.cid=c.cid and s.sid=#{sid}
</select>
<resultMap type="one.to.many.Student" id="getStudentMap">
    <id column="sid" property="sid"></id>
    <result column="sname" property="sname"/>
    <association property="classes" javaType="one.to.many.Classes">
        <id column="cid" property="cid"/>
        <result column="cname" property="cname"/>
    </association>
</resultMap>
```



**多对多**

```xml
<select id="getUsers" resultMap="getGroupMap">
    select g.gid,g.gname from users_groups ug,groups g
    where ug.group_id=g.gid and ug.user_id=#{uid}
</select>
<resultMap type="many.to.many.Groups" id="getGroupMap">
    <id column="gid" property="gid"/>
    <result column="gname" property="gname"/>
    <collection property="users" ofType="many.to.many.Users">
        <id column="uid" property="uid"/>
        <result column="uname" property="uname"/>
    </collection>
</resultMap>
```



#### 15. 实体类中属性名和表中字段名不一致，如何指定

- SQL 语句种定义字段名的别名
- resultMap 映射字段名和实体类属性名



#### 16. 不同的 Xml 映射文件，id 是否可以重复

MyBatis 通过 namespace + id 映射接口方法，如果配置了不同的 namespace 可以重复 id

- namespace 非必须项



#### 17. MyBatis 中执行器

三种基本的 Executor 执行器：

- SimpleExecutor：每执行一次 update 或 select，就开启一个 Statement 对象，用完立刻关闭。
- ReuseExecutor：执行 update 或 select，以 sql 作为 key 查找 Statement 对象。存在就使用，不存在就创建，用完后，不关闭 Statement 对象， 而是放置于 Map。
- BatchExecutor：完成批处理。

通过 ExecutorType 属性指定执行器类型



#### 18. 传递多个参数的方法

- 直接在方法中传递参数，XML 的 SQL 中用 #{参数索引} 获取
- 接口中使用 @param 注解，XML 的 SQL 中用 #{参数名称} 获取



#### 19. resultType 和 resultMap 的区别

- 类的名字和数据库表相同时，可以直接设置 resultType 参数为 Pojo 类
- 类的名字和数据库表不同，需设置 resultMap 将结果名字和 Pojo 名字进行转换



#### 20. Mybatis 比 IBatis 的改进

- MyBatis 接口绑定，注解绑定和 XML 绑定
- 动态 SQL 由节点配置变成 OGNL 表达式
- 关联查询引入 association 和 collection
- IBatis 核心处理类 SqlMapClient；MyBatis 核心处理类 SqlSession



#### 21. 什么是 MyBatis

可以自定义 SQL，存储过程和高级映射的持久层框架



#### 22. MyBatis 好处

- 把 SQL 语句从 Java 代码中独立出来，放在 XML 文件中编写，利于维护
- 封装底层 JDBC 的 API，并将结果集转换成 Java 对象
- 根据数据库特点，灵活编写 SQL，比全自动 ORM 框架 Hibernate 查询效率更高



#### 23. 使用 Mapper 接口的注意事项

- Mapper 接口**方法名**和 mapper.xml 中定义的每个 sql 的 **id** 相同
- Mapper 接口方法的**输入参数类型**和 mapper.xml 中定义的每个 SQL 的 **parameterType** 的类型相同
- Mapper 接口方法的**输出参数类型**和 mapper.xml 中定义的每个 SQL 的 **resultType** 的类型相同
- Mapper.xml 文件中的 **namespace** 是 mapper 接口的**类路径**



#### 24. 自动生成主键

配置文件中设置 usegeneratedkeys 为 true



#### 25. MyBatis 映射 Enum 枚举类

Mybatis 可以映射枚举类，不单可以映射枚举类，Mybatis 可以映射任何对象到表的一列上

映射方式为自定义一个 TypeHandler，实现 TypeHandler 的 setParameter() 和 getResult() 接口方法

TypeHandler 有两个作用，一是完成从 javaType 至 jdbcType 的转换，二是完成 jdbcType 至 javaType 的转换，体现为 setParameter() 和 getResult()两个方法，分别代表设置 SQL 问号占位符参数和获取列查询结果



#### 26. Executor

在 Mybatis 配置文件中，可以指定默认的 ExecutorType 执行器类型，也可以手动给 DefaultSqlSessionFactory 的创建 SqlSession 的方法传递 ExecutorType 类型参数

三种基本的 Executor 执行器

- SimpleExecutor：每执行一次 update 或 select，就开启一个 Statement 对象，用完立刻关闭 Statement 对象
- ReuseExecutor：执行 update 或 select，以 SQL 作为 key 查找 Statement 对象，存在就使用，不存在就创建，用完后不关闭 Statement 对象，而是放置于 Map
- BatchExecutor：完成批处理



#### 27. 一对一，一对多

```xml
<!-- 一对一关联查询 -->
<select id="listClasses" parameterType="int" resultMap="ClassesResultMap">
    select * from classes c,teacher t where c.teacher_id=t.t_id and c.c_id=#{id}
</select>

<resultMap type="com.jourwon.mybatis.pojo.Classes" id="ClassesResultMap">
    <!-- 实体类的字段名和数据表的字段名映射 -->
    <id property="id" column="c_id"/>
    <result property="name" column="c_name"/>
    <association property="teacher" javaType="com.jourwon.mybatis.pojo.Teacher">
        <id property="id" column="t_id"/>
        <result property="name" column="t_name"/>
    </association>
</resultMap>


<!-- 一对多关联查询 -->
<select id="listClasses2" parameterType="int" resultMap="ClassesResultMap2">
    select * from classes c,teacher t,student s where c.teacher_id=t.t_id and c.c_id=s.class_id and c.c_id=#{id}
</select>

<resultMap type="com.jourwon.mybatis.pojo.Classes" id="ClassesResultMap2">
    <id property="id" column="c_id"/>
    <result property="name" column="c_name"/>
    <association property="teacher" javaType="com.jourwon.mybatis.pojo.Teacher">
        <id property="id" column="t_id"/>
        <result property="name" column="t_name"/>
    </association>
    <collection property="studentList" ofType="com.jourwon.mybatis.pojo.Student">
        <id property="id" column="s_id"/>
        <result property="name" column="s_name"/>
    </collection>
</resultMap>
```











