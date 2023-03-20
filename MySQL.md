## MySQL调优

- 性能监控
  - **show profile**：简单的查询剖析工具，查看每个步骤的执行时间
  - **performance schema**：功能完整的监控程序，默认开启，内存存储
  - **show processlist**：监控数据库连接，查看连接的线程个数和状态
- schema与数据类型优化
  - 数据类型优化
  - 合理使用范式和反范式
  - 主键选择
  - 字符集选择
  - 存储引擎选择
  - 适当的数据冗余
  - 适当拆分
- 执行计划
  - **explain** 关键字模拟优化器执行 SQL语句，从而知道 MySQL 是如何处理 SQL 语句的
  - **optimize trace**：进一步知道优化器选择执行方案的过程
- 通过索引进行优化：[Mysql索引优化](https://www.jianshu.com/p/a2222c06db4f)
  -  减少服务器需要扫描的数据量
  - 帮助服务器避免排序和临时表
  - 将随机io变成顺序io 



MySQL层次：

- Client
- [数据库连接池]：减少频繁的开关连接
- Server
  - 连接器：用户连接
  - **分析器**：分析词法和语法
  - **优化器**：优化sql语句，规定执行流程。[基于规则优化 RBO，基于成本优化 CBO]
  - 执行器：实际执行
- 存储引擎



## 面试题

#### 1. 一张表里有 ID 自增主键，insert 了 17 条记录之后，删除了第 15,16,17 条记录后把 Mysql 重启，再 insert 一条记录，这条记录的 ID 是 18 还是 15 ？

表的存储引擎类型不同，结果不同

- **MyISAM**：把自增主键的最大 ID 记录到**数据文件**，重启不会丢失，是18
- **InnoDB**：把自增主键的最大 ID 记录到**内存**，重启或 OPTIMIZE 都会导致丢失，是15



#### 2. Heap 表

存于内存的临时高速缓存，应用于瞬态非关键数据执行，如会话管理或缓存。

- 重启消失
- 使用 Hash 索引不能为 Null，使用 Hash 排序算法查找通过全表扫描，只有表锁，没有行锁
- 不支持可变长数据类型：blob，text，varchar
- 不支持 AUTO_INCREMENT
- 只能使用比较运算符

通过 max_heap_table_size 属性配置最大尺寸



#### 3. MySQL 对比 Oracle 优势

- 开源免费
- 便携
- 带命令提示符的 GUI
- 查询浏览器



#### 4. 区分 FLOAT 和 DOUBLE

FLOAT：4 字节浮点数，8 位精度

DOUBLE：8 字节浮点数，18 位精度



#### 5. CHAR_LENGTH 和 LENGTH

CHAR_LENGTH：字符数

LENGTH：字节数

对于 Latin 字符集两者相同，对于 Unicode 和其他编码不同



#### 6. InnoDB 四种事务隔离级别

- read uncommited ：读到未提交数据，并发最高，一致性最差

- read committed：脏读，不可重复读，最常用的隔离级别

  - 普通select快照读，锁 select / update / delete 会使用记录锁，可能出现不可重复读

- repeatable read：可重读，默认的隔离级别

  - 普通 select 不加锁**快照读**，与之相反的是加悲观锁的**当前读**

    底层 MVCC ( 多版本并发控制器 )，维持一个数据的多个版本，读到的可能是历史版本数据

    为事务分配单向增长的时间戳，形成版本链，在不同事务的写读和读写中并发执行，提升性能

    [数据库MVCC详解](https://www.php.cn/mysql-tutorials-460111.html)

  - 普通select快照读，锁 select / update / delete 根据查询条件情况，会选择记录锁，或者间隙锁 / 临键锁，防止读取到幻影记录

- serializable ：串行事务，并发最差，一致性最好



#### 7. MVCC

Multi-Version Concurrency Control：多版本并发控制，InnoDB 中提高数据库并发性能（ MyISAM 不支持事务）

一般来说，读一行数据会加锁，但 MVCC 在读写冲突时不用加锁

- 当前读：读当前最新的版本，加锁防止修改
- 快照读：基于 MVCC，读到的数据并非最新数据，可能是历史版本的数据



数据库并发场景：

- 读读：没有问题
- 写写：线程安全问题，更新丢失
- 读写：线程安全问题，遇到脏读，幻读，不可重复读

MVCC 为事务分配单向增长的时间戳，关联数据修改的版本，读只读取事务开始前的快照

解决读写并发的场景，脏读，幻读，不可重复读

MVCC + 悲观锁 / 乐观锁，解决读写和写写冲突



MVCC 实现原理：版本链，Undo 日志，Read View

数据库表中的隐藏字段：

- db_trx_id：记录创建这条记录 / 最后一次修改该记录的事务ID。
- db_roll_pointer：回滚指针，，配合 Undo 日志指向上一版本，形成版本链
- db_roll_id：隐藏的自增主键



Undo log：

- 表信息修改前将数据拷贝到 Undo log 中，以便回滚
- 用于 MVCC 的快照读

Read View：

- 执行快照读时生成读视图
- 维护活跃事务的 id
- 做可见性判断，判断当前事务能看到哪个版本的数据
- 属性：
  - trx_ids：活跃的事务版本号集合
  - low_limit_id：创建当前 Read View 时系统最大版本号 + 1
  - up_limit_id：创建当前 Read View 时系统最小版本号
  - create_trx_id：创建当前 Read View 的事务版本号



RC：每个快照读都获取生成最新的 Read View

RR：同一个事务，只有第一个快照读创建 Read View，所以同一事务查询结果每次都是一样的



#### 8. ENUM 用法

字符串列表对象，指定一组预定义的值



#### 9. REGEXP 和 LIKE

REGEXP：正则表达式，匹配整个列包含列值

LIKE：匹配整个列不包含列值，除非加通配符



#### 10. CHAR 和 VARCHAR

VARCHAR 类型用于存储可变长的字符串，适用于字符串列的最大长度比平均长度大的情况。

CHAR 类型用于存储定长的字符串，适用于列的长度为定值的情况，CHAR 值存储时被用空格填充到特定长度。



#### 11. 主键和候选键

候选键是超键的子集，主键是候选键的子集。

超键：能够唯一标识数据的组合

候选键：不含多余属性的超键

主键：用户指定的唯一标识符

外键：在其他标识中的主键



#### 12. Myisamchk

MyISAM 表维护的工具，获取数据库表的信息并检查，修复，优化

- 压缩 MyISAM 表，减少磁盘或内存使用。



#### 13. MyISAM Static 和 Dynamic

Static：所有字段定长，受损情况更易恢复

Dynamic：有 TEXT，BLOB 类型，适应不同长度



#### 14. MySQL 存储引擎

存储引擎称为表类型，数据使用各种技术存储在文件中

技术包括：Storage mechanism，Locking levels，Indexing，Capabilities and functions



FEDERATED：跨服务器访问表



#### 15. 字段设置 AUTO_INCREMENT 在表中达到最大值会发生什么

停止递增，进一步插入会产生错误。

修改主键类型，int 改为 unsigned int 或 bigint



#### 16. 找出最后一次插入时分配的自动增量

LAST_INSERT_ID：不需要指定表的名称



#### 17. 查看索引

```sql
SHOW INDEX FROM <表名> [FROM <数据库名>]
```



#### 18. 通配符中的 % 和 _

%：0 个或更多字符

_：1 个字符



#### 19. MySQL 时戳和 Unix 时戳转换

MySQL 转 Unix：UNIX_TIMESTAMP

Unix 转 MySQL：FROM_UNIXTIME



#### 20. 查询受影响行数

```sql
SELECT COUNT(*) FROM table;
```



#### 21. BLOB 和 TEXT

- BLOB 保存二进制数据，无字符集；TEXT 保存字符数据，有一字符集

- BLOB：TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB

  TEXT：TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB

对 BLOB 值进行排序和比较时区分大小写，对 TEXT 值不区分大小写



#### 22. mysql_fetch_array 和 mysql_fetch_object 的区别

mysql_fetch_array：将结果行作为关联数组返回。

mysql_fetch_object：将结果行作为对象返回。



#### 23. 批处理

```shell
mysql -u username -p password -D Database < .sqlFilePath
```

```sql
source .sqlFilePath
```



#### 24. MyISAM 和 InnoDB

- MyISAM 不支持事务，InnoDB 支持事务

- MyISAM 表级锁，InnoDB 行级锁

- MyISAM 不支持 MVCC，InnoDB 支持 MVCC

- MyISAM 不支持外键，InnoDB 支持外键

- MyISAM 允许无主键，索引为行地址；InnoDB 无主键自动添加主键及索引

- MyISAM 支持全文索引，InnoDB 不支持全文索引

- MyISAM 存储结构

  - .frm：存储表的定义
  - .MYD：数据文件
  - .MYI：索引文件

  InnoDB 所有的表保存在同一数据文件



#### 25. MySQL 存储引擎 / 表格

- MyISAM（默认）
- Heap
- Merge
- InnoDB：事务安全存储引擎，Oracle 开发
- ISAM：索引顺序访问方法，IBM 开发在磁带等辅助存储系统上存储检索数据



#### 26. 优化 DISTINCT

DISTINCT 改为 GROUP BY 并搭配 ORDER BY



#### 27. 输入 16 进制数字

0x



#### 28. 最多创建多少索引列

16



#### 29. NOW() 和 CURRENT_DATE()，TIMESTAMP 默认为 CURRENT_TIMESTAMP 会做什么

NOW()：年月日 时分秒

CURRENT_DATE()：年月日



创建表时 TIMESTAMP 列用 Zero 更新。只要表中的其他字段发生更改，UPDATE CURRENT_TIMESTAMP 修饰符就将时间戳字段更新为当前时间



#### 30. TRIGGERS

- BEFORE INSERT
- AFTER INSERT
- BEFORE UPDATE
- AFTER UPDATE
- BEFORE DELETE
- AFTER DELETE



#### 31. 通用 SQL 函数

数学函数：

- Abs()：求绝对值
- floor()：向下取整
- ceil()：向上取整
- FORMAT(X, D)：格式化数字 X 到 D 有效数字

字符串函数：

- insert(s1,index,length,s2)：替换函数，被替换的字符串，替换的位置，替换长度，用于替换的字符串
- upper(str)，ucase(str)：大写
- lower(str)，lcase(str)：小写
- left(str,length)：返回字符串左边的定长字符
- right(str,length)：返回字符串右边的定长字符
- substring(str,index,length)：返回字符串从索引位置起的定长字符
- reverse(str)：倒序输出
- CONCAT(A, B)：连接两个字符串值以创建单个字符串输出。

日期函数：

- CURDATE()，CURTIME()，CURRDATE() , CURRTIME()：返回当前日期或时间
- NOW()：将当前日期和时间作为一个值返回
- DATEDIFF(A，B)：确定两个日期之间的差异，通常用于计算年龄
- MONTH()，DAY()，YEAR()，WEEK()，WEEKDAY()：从日期值中提取给定数据
- HOUR()，MINUTE()，SECOND()：从时间值中提取给定数据
- FROMDAYS(INT)：将整数天数转换为日期值
- adddate(date,num)：返回 date 日期后 num 天数的日期
- subdate(date,num)：返回 date 日期前 num 天数的日期
- SUBTIMES(A，B)：确定两次之间的差异

聚合函数：

- Count(字段)：根据某个字段统计总记录数
- sum(字段)：计算某字段数值总和
- avg(字段)：计算某字段数值均值
- Max(字段)，Min(字段)：某个字段的最大最小值



#### 32. 记录货币的类型

NUMERIC 和 DECIMAL



#### 33. 数据表损坏情况

- 服务器断电
- 未关闭 MySQL 情况下强制关机



#### 34. MySQL 中的权限表

存放在 mysql 数据库中的表：

- user
- db
- table_priv
- columns_priv
- host



#### 35. 锁

MyISAM 支持表锁，InnoDB 支持表锁和行锁

- 表级锁：开销小，加锁快，不会出现死锁。锁定粒度大，发生锁冲突的概率高，并发度最低
- 行级锁：开销大，加锁慢，会出现死锁。锁粒度小，发生锁冲突的概率小，并发度最高



#### 36. 三范式

- 1 NF：列，字段原子性，不可再分割
- 2 NF：行，每行唯一区分
- 3 NF：表，表信息除主键外不重叠



#### 37. 优化方法

- JDBC 中使用 **PrepareStatement** 优于 **Statement**。
  - Statement 用于执行**静态 SQL 语句**，执行时必须指定一个事先准备好的 SQL 语句。
  - PrepareStatement 中，SQL 语句代表某一类操作，被**预编译**并保存在对象中，语句中可以包含动态参数 “ ? ” ，执行时为 “ ? ” 动态设置参数值。
  - 使用 PrepareStatement 对象时，SQL 被数据库进行解析和编译，然后被放到命令缓冲区，每当执行同一个PrepareStatement 对象，SQL 就被解析一次，但不会再次编译。在缓冲区的预编译命令可以重用。
  - PrepareStatement 通过减少编译次数提高数据库性能。
- 外键约束会影响性能，设计数据库时保证数据完整性的情况下去除外键
- 适当冗余，减少连表查询
- 合并数据结果集时，UNION ALL 不去重不排序比 UNION 去重排序快很多



#### 38. 索引种类

索引：以空间换时间，帮助 MySQL 高速获取数据的数据结构，对表中一列或多列值进行排序

通常使用 B 树或 B+ 树

- 普通索引：针对数据库表创建索引
- 唯一索引：与普通索引类似，不同的就是 MySQL 数据库索引列的值必须唯一，但允许空值
- 主键索引：一种特殊的唯一索引，不允许有空值。一般是在建表的 时候同时创建主键索引
- 组合索引：进一步榨取 MySQL 效率，考虑建立组合索引，将数据库表中的多个字段联合起来作为一个组合索引



#### 39. MySQL 集群主从复制

MySQL 集群一主多从，将主机数据复制到从机上

日志文件，日志循环

- 主服务器更新记录到二进制日志文件。
- 从服务器把主服务器的二进制日志拷贝到自己的中继日志（replay log）。
- 从服务器更新中继日志的时间， 并更新应用到自己的数据库。



复制方法：

- 基于语句的复制
- 基于行的复制
- 基于语句和行的混合复制



#### 40. 表中有大字段，且读多写少

将字段拆分成子表，大字段拆成小字段，提高查询效率。



#### 41. InnoDB 的行锁如何实现

InnoDB 在索引项上加锁实现行锁，即只有通过索引检索加行锁，否则使用表锁

Oracle 加锁数据行实现行锁



#### 42. 控制内存分配的全局参数

- Keybuffersize：**索引缓冲区**大小，只对 MyISAM 有效

  Keyreadrequests 和 Keyreads：比值尽可能高，响应率高

  性能好的机器设置多个 Keybuffer 缓存专门的索引

- innodbbufferpool_size：**缓冲池**字节大小，InnoDB 缓存表索引数据的内存区，减少磁盘读取

- querycachesize：**查询语句缓冲区**。每条查询 SQL 都会有 Hash 值，MySQL 收到查询 SQL 先去 querycache 里匹配

- readbuffersize：**读入缓冲区**大小。对表顺序扫描的请求分配读入缓冲区



#### 43. SELECT * 和 SELECT 全部字段有何区别

- 前者要**解析数据字典**，后者不需要
- 前者**输出顺序**与建表列顺序相同，后者按指定字段顺序。
- **表字段修改**后，前者不需要修改，后者需要做出相应修改
- 前者无法**优化**，后者可以建立索引进行优化
- 前者**可读性**差，后者可读性好 



#### 44. HAVING 和 WHERE

查询执行顺序：from > where > group（含聚合）> having > order > select

- WHERE 返回结果前起作用，可以使用索引，不能使用聚合函数
- HAVING 对结果集进行过滤，不能使用索引，可以使用聚合函数



#### 45. 不存在插入，存在则更新

ON DUPLICATE KEY UPDATE

```sql
INSERT INTO table (a,b,c) VALUES (1,2,3) ON DUPLICATE KEY UPDATE c=c+1;
```



#### 46. 优化经验

- MySQL 开启查询缓存进行优化，但是缓存对函数不起作用，需要使用变量接收查询值进行缓存
- 使用 EXPLAIN + SQL 语句，分析查询语句的执行过程，优化语句和表结构
- 只需要一条数据时使用 LIMIT 1，在找到一条数据后就停止搜索
- 创建索引：
  - UNIQUE ( 唯一索引 )：不可以出现相同的值，可以有NULL值
  - INDEX ( 普通索引 )：允许出现相同的索引内容
  - PROMARY KEY ( 主键索引 )：不允许出现相同的值
  - fulltext index ( 全文索引 )：针对值中的某个单词，效率很低
  - 组合索引：将多个字段建到一个索引里，列值的组合必须唯一
- 连表查询时，Join 的条件字段类型相同且为索引
- 打乱数据不要用 ORDER BY RAND()，RAND() 函数很消耗 CPU 资源
- 避免 SELECT *，需要什么取什么
- 为每张表设置 ID 主键，使用 UNSIGNED INT 类型
- 固定范围的字符串字段，使用 ENUM 代替 VARCHAR
- PROCEDURE ANALYSE() 获取优化建议
- 尽量使用 NOT NULL，NULL 不仅需要额外的空间还会带来更复杂的运算
- 用 UNSIGNED INT 存放 IP 地址
- 固定长度的表查询速度更快
- DELETE 和 INSERT 语句加表锁，将大语句拆分成小语句防止过度阻塞
- 使用 ORM ( Object Relational Mapper ) 对象关系映射器
- 永久连接可以减少重连次数，但是会导致资源问题，谨慎使用



#### 47. 列的字符串类型有哪些

SET

BLOB

ENUM

CHAR

TEXT

VARCHAR



#### 48. MySQL 关键字

添加索引：alter table tableName add 索引 (索引字段)

主键：primary key

唯一：unique

全局：fulltext

普通：index

多列索引：index index_name



#### 49. 导入导出

```powershell
# 导出所有数据库
mysqldump -u [username] -p -A>[filePath]

# 导出数据和数据结构
mysqldump -u [username] -p [DBname]>[filePath]

# 导出数据不含数据结构
mysqldump -u [username] -p -t [DBname]>[filePath]

# 导出数据库结构
mysql -hlocalhost -u username -p -d --add-drop-table DBname> filePath

##############################

# 导入SQL脚本并执行
mysql -u username -p DBname < x.sql

# 备份（多个）数据库
mysqldump -h hostname -u username -p password [-databases] DBname [DBname2 ...]> x.sql

# 压缩备份
mysqldump -h hostname -u username -p password DBname | gzip > x.sql.gz

# 备份一些表
mysqldump -h hostname -u username -p password DBname table1 table2 > x.sql

# 备份所有数据库
mysqldump --all-databases  x.sql

# 还原数据库
mysql -h hostname -u username -p password DBname < x.sql

# 还原被压缩的数据库
gunzip < x.sql.gz | mysql -u username -p password DBname
```



#### 50. truncate，drop，delete

- truncate ( DDL 语句 )：只针对于删除表的操作，直接删除原来的表，再重新创建一个一模一样的新表
- drop ( DDL 语句 )：不可逆操作，将表所占用空间全部释放掉
- delete ( DML 语句 )：可以删除表也可以删除行，但是删除记录会被计入日志保存，且表空间大小不会恢复到原来

执行速度：drop > truncate > delete















