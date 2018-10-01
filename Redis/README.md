### Godis
* 什么是Gedis
  * Go语言操作redis的工具

=====================================================================================

### 慢查询

=====================================================================================

### pipeline
1. 什么是pipeline
pipleline就是流水线操作，将多条命令打包，再进行发送，最后批量获取结果
1次pipeline = 1次网络时间 + n次命令时间
2. Jedis使用pipeline
​```
//没有使用pipeline
Jedis jedis = new Jedis("127.0.0.1",6379);
for(int i=0; i<10000; i++) {
    jedis.hset("hashkey:"+i,"field"+i,"value"+i);
}
​```
​```
//使用pipeline
Jedis jedis = new Jedis("127.0.0.1",6379);
for(int i=0; i<100; i++) {
    Pipeline pipline = jedis.piplined();
	for(int j=i*100; j<(i+1) * 100; j++) {
	    pipline.hset("hashkey:"+j,"field"+j,"value"+j);
	}
	pipeline.syncAndReturnAll();
}
​```
3. 注意
pipeline不是原子操作，因为redis会对pipeline进行拆分
但是结果是有序的

=========================================================

### 发布订阅
1. 三个角色
发布者：位于redis-cli
订阅者：位于redis-cli
频道：位于redis-server

2. API
(1) publish
【格式】publish channel message
【功能】发布信息
【例子】publish nar.tv "hello"
(2) subscribe
【格式】subscribe [channels]
【功能】订阅频道
【例子】subscribe nar.tv 
(3) unsubcribe
【格式】unbsubcribe [channels]
【功能】取消订阅
【例子】unsubcribe nar.tv

===========================================================

### bitmap
1. 什么是位图
位图就是通过一个bit位来表示某个元素对应的值或者状态，简而言之就是位操作

2. 功能
极大节省存储空间

3. API
(1) setbit
【格式】setbit key offset value
【功能】设置key的位图
【例子】setbit name 0 1
(2) getbit
【格式】getbit key offset
【功能】获取key对应位置的bit
【例子】getbit name 0
(3) bitop
【格式】bitop [op] bitmap3 bitmap1 bitmap2
【功能】获取两个bitmap的交集|并集|异或|非
【注意】交集and；并集or；异或xor；非not
【例子】bitop and newMap bitmap1 bitmap2

============================================================

### HyperLogLog
1. 什么是HyperLogLog
HyperLogLog就是基于HyperLogLog算法，以极小空间完成独立数量统计
本质还是string

2. API
(1) pfadd
【格式】pfadd key element [element...]
【功能】向hyperloglog中添加元素
(2) pfcount
【格式】pfcount key
【功能】计算hyperloglog的独立总数
(3) pfmerge
【格式】pfmerge destkey sourcekey [sourcekey]
【功能】合并多个hyperloglog

3. 缺点
(1) 错误率：0.81%
(2) 不能取出内容，只能做统计

=============================================================



=============================================================================

### 主从复制
1. 概念
主从复制，是用来建立一个和主数据库完全一样的数据库环境，称为从数据库

2. 作用
(1) 数据副本
(2) 读写分离

3. 注意
(1) 一个master可以有多个slave，而一个slave只能有一个master
(2) 数据流向是单向的：master --> slave
(3) 当一个节点成为从节点后，该节点上原来的数据全部被清楚

4. 使用主从复制的方式
(1) slaveof命令
创建从节点
redis-master> slaveof 127.0.0.1 6379
取消从节点
redis-slave> slaveof no one
(2) 通过配置redis.conf
slaveof ip port      #设置从节点的ip和端口
slave-read-only yes  #设置只能读
(3) 比较
|方式|命令|配置|
|优点|无需重启|统一配置|
|缺点|不便于管理|需要重启|

5. 全量复制
(1) 原理
![全量复制](全量复制原理.png)
(2) 开销
(1) bgsave时间
(2) RDB文件传输时间
(3) 从节点清除原始数据的时间
(4) 从节点加载RDB的时间
(5) 可能的AOF重写时间

6. 部分复制
![部分复制](部分复制原理.png)

7. 常见问题
(1) 读写分离
(2) 复制数据延迟
(3) 读取过期数据
(4) 从节点故障

### 主从复制实现故障转移
图片

==========================================

### redis-sentinel
1. 背景
对于一个master-slave主从复制集群，应该监控集群，当发现master宕机时，自动从slave中选出一个master
但是redis和redis的客户端都没有实现这个功能，需要用户手动实现，太过于麻烦和困难
所以redis官方推出了redis-sentinel程序，它有一个独立运行的进程，用于监控master-slave集群，发现master宕机后，自动切换

2. redis-sentinel功能
(1) 管理master-slave集群，并监控其中的redis节点是否正常运行
(2) 如果发现某个redis节点发生异常后，能够通知另外一个进程
(3) 当master发生错误后，能够自动选举新的master，并完成主从复制
(4) 客户端能够从sentinel程序获取master-slave集群中的信息【即作为客户端和master-slave集群之间的桥梁】

3. redis-sentinel集群
因为sentinel本身是一个redis，也有出现问题的可能性,只使用单个sentinel进程来监控redis集群是不可靠的
当sentinel进程出现问题后，master-slave集群系统将无法按照预期的方式运行
所以有必要将sentinel集群，即同时用多个sentinel监控一个master-slave集群，这样有几个好处
(1) 即使有一些sentinel进程宕掉了，依然可以进行redis集群的主备切换
(2) 如果有多个sentinel，redis的客户端可以随意地连接任意一个sentinel来获得关于redis集群中的信息

4. redis-sentinel配置
静态配置保存于一个独立文件中，内容包括
(所有的配置都可以在运行时用命令sentinel set command动态修改)
​```
sentinel本身是一个redis，所以有redis的基础配置

daemonize yes        # 使用守护线程
port 23679           # 占用端口号
dir "~/data"         # 工作目录
logfile "26379.conf" # 日志名称

sentinel特有的配置

sentinel监控的master的名字叫做mymaster,地址为127.0.0.1:6379

2代表当sentinel集群中有两个sentinel认为master宕机时，才真正认为该master不可用

sentinel monitor mymaster 127.0.0.1 6379 2

sentinel会向master发送心跳PING来确认master是否存活

如果master在“down-after-milliseconds”内不回应PONG,或者是回复了一个错误消息

那么这个sentinel会认为这个master已经不可用了

sentinel down-after-milliseconds mymaster 6000

## 
sentinel failover-timeout mymaster 18000

在发生主备切换时，这个选项指定了最多可以有多少个slave同时对新的master进行同步

这个数字越小，完成切换所需的时间就越长，但是如果这个数字越大，就意味着越多的slave因为replication而不可用

可以通过将这个值设为 1 来保证每次只有一个slave处于不能处理命令请求的状态。

sentinel parallel-syncs mymaster 1
​```

5. 运行redis-sentinel
​```
src/redis-sentinel conf/sentinel-26379.conf
​```

6. sentinel状态持久化
snetinel的状态会被持久化地写入sentinel的配置文件中
每次当收到一个新的配置时，或者新创建一个配置时，配置会被持久化到硬盘中，并带上配置的版本戳
这意味着，可以安全的停止和重启sentinel进程。

7. sentinel故障转移流程

8. 客户端连接redis-sentinel
客户端实现基本原理
(1) client由地址集合遍历sentinel节点集合，获取一个可用的sentinel节点
(2) 找到可用的sentinel节点后，通过该sentinel节点获取master节点
(3) 获取master节点后，会使用role来验证节点是不是真的master
(4) client获取由sentinel发送过来的数据节点变化通知

客户端使用流程
(1) 获取sentinel地址集合
(2) 获取masterName

Jedis使用
​```
JedisSentinelPool sentinelPool = new JedisSentinelPool(masterName,sentinelSet,poolConfig,timeout);

Jedis jedis = null;
try {
    jedis = sentinelPool.getResource();
} catch (Exception e) {
    logger.error(e.getMessge(),e);
} finally {
    if(jedis != null) {
	    jedis.close();
	}
}
​```

===============================================================

redis-cli 使用

redis-cli -p port #连接到指定端口的redis中
redis-cli -p port shutdown #关闭指定端口的redis-server
redis-cli -p port info replication #查看指定端口的redis信息
redis-cli> info #查看当前连接的redis-server的信息