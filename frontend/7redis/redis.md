## 有没有了解过Redis？

##### 1 用Redis实现缓存：将热门数据和热门页面存到Redis进行缓存，比如热门商品信息，商品页面和网站首页。

##### 2 缓存遇到的问题：缓存穿透、缓存雪崩、缓存击穿。

##### 3 Redis的进阶功能：Redis有各种数据结构，除了缓存之外，还能实现很多功能。比如：消息队列、附近的人、排行榜等等。

##### 4 Redis持久化：Redis可以将缓存持久化到本地，持久化策略包括RDB和AOF。

##### 5 集群：如果单机Redis不够用的话，可以考虑搭建Redis集群，Redis集群有主从和哨兵两种模式。



# Redis如何做持久化的？能说一下RDB和AOF的实现原理吗？

Redis官方提供了不同级别的持久化方式：

- **RDB 持久化**：
  - RDB 持久化通过在指定时间间隔内对数据进行快照（snapshot）来实现持久化。快照是对数据库中所有键值对的一个完整的备份。
  - 当启用 RDB 持久化时，Redis 会周期性地将数据集保存到磁盘上的一个 RDB 文件中。这个过程是通过将数据集写入一个临时文件，然后用这个临时文件替换原来的 RDB 文件来实现的。
- **AOF 持久化**：
  - AOF 持久化记录了服务器接收的每个写操作（写命令），并将这些写操作追加到文件的末尾。因此，AOF 文件包含了重建数据集的所有写操作。
  - AOF 持久化提供了更好的数据安全性，因为它可以在数据发生变化时记录每个写操作。然而，AOF 文件通常比 RDB 文件大，并且在写入时可能会导致性能损失。

# 讲解一下Redis的线程模型？

redis 内部使用文件事件处理器 file event handler，这个文件事件处理器是单线程的，所以 redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，根据 socket 上的事件来选择对应的事件处理器进行处理。

如果面试官继续追问为啥 redis 单线程模型也能效率这么高？

- 纯内存操作
- 核心是基于非阻塞的 IO 多路复用机制
- 单线程反而避免了多线程的频繁上下文切换问题

## **什么是缓存雪崩？**

缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，请求直接落到数据库上，引起数据库压力过大甚至宕机。和缓存击穿不同的是，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。

**缓存雪崩解决方案**

常用的解决方案有：

- 均匀过期

## 那你了解缓存穿透和击穿么，可以说说他们跟雪崩的区别么？ 

嗯，了解，我先说一下缓存穿透吧，缓存穿透是指缓存和数据库中都没有的数据，而用户不断发起请求，我们数据库的 id 都是1开始自增上去的，如发起为id值为 -1 的数据或 id 为特别大不存在的数据。这时的用户很可能是攻击者，攻击会导致数据库压力过大，严重会击垮数据库。



## 如何解决呢

事前：**Redis** 高可用，主从+哨兵，**Redis cluster**，避免全盘崩溃。

事中：本地 **ehcache** 缓存 + **Hystrix** 限流+降级，避免** MySQL** 被打死。

事后：**Redis** 持久化 **RDB**+**AOF**，一旦重启，自动从磁盘上加载数据，快速恢复缓存数据。



# 什么是Redis主从复制

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master)，后者称为从节点(slave)；数据的复制是单向的，只能由主节点到从节点。

**主从复制的作用**

- 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
- 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
- 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务，分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
- 高可用基石：主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。



# Sentinel（哨兵模式）的原理你能讲一下吗？

**哨兵模式的原理**

哨兵模式的主要作用在于它能够自动完成故障发现和故障转移，并通知客户端，从而实现高可用。哨兵模式通常由一组 Sentinel 节点和一组（或多组）主从复制节点组成。

**心跳机制**

基于 Redis 的订阅发布功能， 每个 Sentinel 节点会向主节点的 **sentinel**：hello 频道上发送该 Sentinel 节点对于主节点的判断以及当前 Sentinel 节点的信息 ，同时每个 Sentinel 节点也会订阅该频道， 来获取其他 Sentinel 节点的信息以及它们对主节点的判断。

**故障转移**

每个 Sentinel 都会定时进行心跳检查，当发现主节点出现心跳检测超时的情况时，此时认为该主节点已经不可用，这种判定称为**主观下线**。



# 假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？



```plain
使用keys指令可以扫出指定模式的key列表：
keys pre*
```

# 如果有大量的key需要设置同一时间过期，一般需要注意什么？



如果大量的key过期时间设置的过于集中，到过期的那个时间点，Redis可能会出现短暂的卡顿现象(因为redis是单线程的)。严重的话可能会导致服务器雪崩，所以我们一般在过期时间上加一个随机值，让过期时间尽量分散。