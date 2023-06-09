## 面试题

#### 1. Kafka 的设计

- **producers** 通过网络将消息发送到 **Kafka 集群**，集群向 **consumer** 提供消息
- Kafka 以集群的方式运行，由一个或多个服务组成，每个服务叫做一个 **broker**
- 消息以 **topic** 为单位进行归纳，发布消息的是 **producers**，消费消息的是 **consumer**



#### 2. 数据传输事务级别

- 最多一次：消息不会被重复发送，最多被传输一次，但有可能一次也不传输
- 最少一次：消息不会被漏发送，最少被传输一次，但有可能被重复传输
- 精确一次：Exactly Once，恰好传输一次



#### 3. Kafka 判断节点存活的条件

- 节点和 Zookeeper 连接，Zookeeper 通过心跳机制检查每个节点的连接
- 节点是 follower，要求及时同步 leader 的写操作



#### 4. producer 消息发送

producer 直接将数据发送到 broker 的 leader ( 主节点 )。

所有的 Kafka 节点都可以告知 producer ：哪些节点是活动的，目标 topic 目标分区的 leader 在哪。



#### 5. consumer 消息消费

consumer 消费消息时，向 broker 发出 " fetch " 请求去消费特定分区的消息

指定消息在日志中的偏移量（offset），可以从指定位置进行消费，拥有 offset 控制权可以向后回滚重新消费之前的消息



#### 6. Pull 和 Push 模式

- producer 将消息 push 到 broker
- consumer 从 broker 中 pull 消息

若让 broker 决定消息的推送速度，容易导致 consumer 端的崩溃，Kafka 让 consumer 依据自己的消费能力决定消费速度

同时，pull 模式当 broker 中没有消息可消费时，consumer 会自我阻塞直到新消息到达，不会一直轮询



#### 7. 消息存储格式

- 消息长度 4 bytes
- 版本号 1 bytes
- CRC 校验码 4 bytes
- 具体消息 n bytes



#### 8. 高效存储的设计特点

- topic 的分区中，大文件被分成多个小文件段，定期清理减少磁盘占用
- 通过索引快速定位 message 并确定返回的大小
- 索引映射到内存中，减少磁盘 IO
- 索引文件稀疏存储，降低占用空间



#### 9. Kafka 和其他传统消息中间件的区别

- 持久化日志，可以被重复读取和无限期保留
- 分布式系统，以集群的方式运行，灵活伸缩，内部通过复制数据提升容错能力，保证高可用
- 支持实时的流式处理



#### 10. 分区副本分配

- 副本因子不能大于 Broker 的个数
- 第一个分区（编号为 0）的第一个副本放置位置随机从 brokerList 选择
- 其他分区的第一个副本放置位置相对于第 0 个分区依次后移
- 剩余的副本相对于第一个副本放置位置由 nextReplicaShift 随机分配



#### 11. 新建的分区在哪个目录下存放数据

**log.dirs** 参数配置数据存放目录。

配置了多个目录时，Kafka 在**含有分区目录最少的文件夹**中创建新的分区目录，名为 Topic + 分区 ID



#### 12. 分区消息保存到磁盘

分区消息以文件夹的形式保存到 broker，每个分区序号从 0 递增，且消息有序。

分区文件下有多个 segment（xxx.index，xxx.log），segment 文件大小和配置文件大小一致默认为 1g，如果大于 1g 会滚动一个新的 segment ，并以上一个 segment 最后一条消息的偏移量命名。



#### 13. ack 机制

producer，leader，follower

消息发送确认机制，ack 有三个值：0，1，-1

- 0：生产者不会等待 broker 的 ack，延迟最低，存储的保证最弱
- 1：leader 接收到消息后发送 ack。如果 leader 挂掉后不确保是否复制完成，新 leader 也会导致数据丢失
- -1：leader 等所有的 follower 收到副本数据后才会发出的 ack，数据不会丢失



#### 14. 分区与消费

**分区Partition是最小的并行单位**

- 一个消费者可以消费多个分区
- 一个分区可以被多个消费者组里的消费者消费
- 一个分区不能被同一个消费者组里的多个消费者消费



**点对点（一对一）模式：**

- 所有消费者都属于同一个消费者组



**分区与消息顺序：**

- 同一个生产者发送到同一分区的消息，先发送的offset比后发送的小
- 同一生产者发送到不同分区的消息，消息顺序无法保证

**分区与消费顺序：**

- 消费者按照消息在分区里的存放顺序进行消费
- Kafk只保证分区内消息顺序，不能保证分区间消息顺序

只设置一个分区，可以保证所有消息有序，但是失去拓展性和性能

可以设置消息的key，相同key的消息发送到同一分区























