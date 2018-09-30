```
#### redis是什么
开源，免费，高性能的单线程的键值对的缓存系统

#### redis典型使用场景
1. 缓存系统
2. 计数器
3. 消息队列系统
4. 排行榜
5. 社交网络
6. 实时系统

#### redis下载与安装
1. 下载redis源码压缩包
wget http://download.redis.io/releases/redis-3.0.7.tar.gz
2. 解压缩
tar -xzf redis-3.0.7.tar.gz
3. 进行编译安装
cd redis
make && make install

#### 启动与关闭redis服务端
1. 最简启动
${redis}/src/redis-server
2. 配置文件启动【推荐】
${redis}/src/redis-server config.conf
3. 动态参数启动
${redis}/src/redis-server --port 6380
4. 检查是否成功启动
ps -ef | grep redis
5. 关闭
${redis}/src/redis-cli shutdown

#### 启动与关闭redis客户端
1. 启动
${redis}/src/redis-cli
2. 关闭
exit

#### redis配置文件
1. daemonize 是否以守护进程方式启动【默认no】
2. port redis占用端口【默认6379】
3. logfile redis系统日志
4. dir redis工作目录

=================================================

#### redis通用命令
1. keys [pattern]
    获取数据库中所有符合pattern的key
	【时间复杂度】：O(n)
2. dbsize
    获取数据库的大小
	【时间复杂度】：O(1)
3. exists key
    判断是否存在key
    存在返回1，不存在返回0
	【时间复杂度】：O(1)
4. del key [key...]
    删除key
	返回删除个数
	【时间复杂度】：O(1)
5. expire key seconds
    设置key在seconds秒后过期
	【时间复杂度】：O(1)
6. ttl key
    查看key的剩余过期时间
	-1 表示永远不过期
	【时间复杂度】：O(1)
	-2 表示key已经不存在了
7. persist key
    去掉key的过期时间
8. type key
    判断key的数据类型

==============================================

#### 字符串
1. 结构
key      -->  value
----------------------
hello    -->  world
counter  -->    1
2. 注意
value最大512MB
3. API
(0) 总结
|命令|命令|命令|命令|
|---|---|---|---|
|set|setnx|setxx|mset|
|get|mget|||
|del||||
|incr|incrby|incrbyfloat||
|decr|decrby|decrbyfloat||
|append|strlen|||
(1) get
【格式】get key
【功能】获取key对应的value
【注意】不存在key，则返回(nil)
【时间复杂度】O(1)
(2) set
【格式】set key value
【功能】新建/更改key-value
【注意】不管存不存在都设置
【时间复杂度】O(1)
(3) del
【格式】del key
【功能】删除key-value
【时间复杂度】O(1)
(4) incr
【格式】incr key
【功能】key自增1
【注意】如果key不存在，则创建并返回1；如果key不能自增，则报错
【时间复杂度】O(1)
(5) decr
【格式】decr key
【功能】key自减1
【注意】如果key不存在，则创建并返回-1；如果key不能自减，则报错
【时间复杂度】O(1)
(6) incrby
【格式】incrby key k
【功能】key自增k
【注意】如果key不存在，则创建并返回k；如果key不能自增，则报错
【时间复杂度】O(1)
(7) decrby
【格式】decrby key k
【功能】key自减k
【注意】如果key不存在，则创建并返回-k；如果key不能自减，则报错
【时间复杂度】O(1)
(8) setnx
【格式】setnx key value
【功能】新建key-value
【注意】如果key存在，则不能进行操作
【时间复杂度】O(1)
(9) setxx
【格式】setxx key value
【功能】更新key-value
【注意】如果key不存在，则不能进行操作
【时间复杂度】O(1)
(10) mset
【格式】mset key1 value1 key2 value2 ... keyn valuen
【功能】批量新建/更新key-value
【注意】如果key不存在，则新建；如果key存在，则更新
【时间复杂度】O(n)
(11) mget
【格式】mget key1 key2 ... keyn
【功能】批量获取key-value
【注意】如果key不存在，则返回(nil)
【时间复杂度】O(n)
(12) getset
【格式】getset key value
【功能】先获取key-value，再更新key-value
【注意】如果key不存在，则返回(nil)，后创建key-value
【时间复杂度】O(1)
(13) append
【格式】append key value
【功能】追加value
【注意】如果key不存在，则创建新key-value
【时间复杂度】O(1)
(14) strlen
【格式】mget key1 key2 ... keyn
【功能】批量获取key-value
【注意】如果key不存在，则返回(nil)
【时间复杂度】O(1)
(15) incrbyfloat
【格式】incrbyfloat key floatnum
【功能】使key自增floatnum
【注意】如果key不存在，则返回新key-value
【时间复杂度】O(1)
(16) decrbyfloat
【格式】decrbyfloat key floatnum
【功能】使key自减floatnum
【注意】如果key不存在，则返回新key-value
【时间复杂度】O(1)
(17) getrange
O(1)
(18) setrange
O(1)

===========================================

#### Hash结构
1. 结构
key  -->  |field | value
----------------------------
          |name  | narlinen
user -->  |age   | 20
          |id    | 00
2. 特点
可以看做small redis
3. API
(0) 总结
|命令|命令|命令|
|---|---|---|
|hset|hmset||
|hget|hmget||
|hdel|||
|hexists|hlen||
|hgetall|hkeys|hvals|
(1) hget
【格式】hget key field
【功能】获取key对应field的value
【注意】如果field不存在，则返回(nil)
【时间复杂度】O(1)
(2) hset
【格式】hset key field value
【功能】设置key对应field的vlaue
【注意】如果key不存在，则新建field-value
【时间复杂度】O(1)
(3) hdel
【格式】hdel key field
【功能】删除key对应的field-value
【注意】如果key不存在，则返回0
【时间复杂度】O(1)
(4) hexists
【格式】hexists key field
【功能】判断是否存在field
【注意】如果不存在，则返回0；如果存在1
【时间复杂度】O(1)
(5) hlen
【格式】hlen key
【功能】获取field的数量
【注意】如果key不存在，则返回0
【时间复杂度】O(1)
(6) hmget
【格式】hmget key field1 field2 ... fieldN
【功能】批量返回filed对应的value
【注意】如果key不存在，则返回nil
【时间复杂度】O(n)
(7) hmset
【格式】hmset key field1 value1 field2 value2 ... fieldN valueN
【功能】批量设置field-value
【注意】如果field不存在，则创建新field-value
【时间复杂度】O(1)
(8) hgetall
【格式】hgetall key
【功能】获取所有的field和value
【时间复杂度】O(n)
(9) hkeys
【格式】hkeys key
【功能】获取所有的field
【时间复杂度】O(n)
(10) hvals
【格式】hvals key
【功能】获取所有的value
【时间复杂度】O(n)
(11) hsetnx
【格式】hsetnx key field value
【功能】当不存在field时进行设置
【时间复杂度】O(1)
(12) hincrby
【格式】hincrby key field num
【功能】使field对应value自增num
【注意】如果field不存在，则创建新field-value，其value为num；如果不能自增则报错
【时间复杂度】O(1)
(13) hincrbyfloat
【格式】hincrbyfloat key field floatnum
【功能】使field对应value自增floatnum
【注意】如果field不存在，则创建field-value，其value为floatnum；如果不能自增则报错
【时间复杂度】O(1)

===================================================

#### List
1. 结构
   key   -->   elements
username -->  a-b-c-a-b-c
2. 特点
元素重复，有序
3. API
(1) rpush
【格式】rpush key value1 value2 ... valueN
【功能】从列表右侧插入值(1~N个)
【时间复杂度】O(1～N)
(2) lpush
【格式】lpush key value1 value2 ... valueN
【功能】从列表左插入值(1~N个)
【时间复杂度】O(1～N)
(3) linsert
【格式】linsert key before|after value newValue
【功能】从列表第一个value的前|后插入新元素newValue
【时间复杂度】O(n)
(4) lpop
【格式】lpop key
【功能】从列表左侧弹出第一个item
【时间复杂度】O(1)
(5) rpop
【格式】rpop key
【功能】从列表右侧弹出第一个item
【时间复杂度】O(1)
(6) lrem
【格式】lrem key count value
【功能】根据count的值，从列表中删除count个等于value的项
【注意】count>0，从左到右；count<0，从右到左；couont=0，删除所有
【时间复杂度】O(n)
(7) ltrim key start end
【格式】ltrim key start end
【功能】按照索引范围修剪列表
【注意】保留[start,end]的元素，即是左闭右闭区间
【时间复杂度】O(n)
（8）lrange key start end
【格式】lrange key start end
【功能获取列表中指定索引范围所有的item
【注意】查找[start,end]的元素，即是左闭右闭区间
【时间复杂度】O(n)
(9) lindex key index
【格式】lindex key index
【功能】获取列表中指定索引的item
【注意】支持反向索引，即负数
【时间复杂度】O(n)
(10) llen key
【格式】llen key
【功能】获取列表的长度
【时间复杂度】O(1)
(11) lset key index newValue
【格式】lset key index newValue
【功能】修改列表中index下标的元素为newValue
【注意】支持反向索引，即负数
【时间复杂度】O(n)
(12) blpop
【格式】blpop key timeout
【功能】阻塞timeout秒后弹出左侧元素
【时间复杂度】O(1)
(13) brpop
【格式】brpop key timeout
【功能】阻塞timeout秒后弹出右侧元素
【时间复杂度】O(1)

==============================================

#### Set
1. 结构
  key   -->            values
subject --> chinese,music,art,...,maths 
2. 特点
元素不重复，无序
3. API
(1) sadd
【格式】sadd key element
【功能】向集合中添加元素element
【注意】如果element已存在，则添加失败
【时间复杂度】O(1)
(2) srem
【格式】srem key element
【功能】将集合中key的element删除
【时间复杂度】O(1)
(3) scard
【格式】scard key
【功能】获取集合中元素个数
【时间复杂度】O(n)
(4) sismember
【格式】sismember key item
【功能】判断item是否为集合中的元素
【时间复杂度】O(1)
(5) srandmember
【格式】srandmember key
【功能】随机取出集合中的一个元素
【注意】是取出不是弹出，即不会破坏的原有集合
【时间复杂度】O(1)
(6) smembers
【格式】smembers key
【功能】获取集合中所有的元素
【时间复杂度】O(n)
(7) spop
【格式】spop key
【功能】从集合中随机弹出一个元素
【时间复杂度】O(1)
(8) diff
【格式】sdiff set1 set2
【功能】求两个集合的差集
【时间复杂度】O(n)
(9) sinter
【格式】sinter set1 set2
【功能】求两个集合的交集
【时间复杂度】O(n)
(10) sunion
【格式】sunion set1 set2
【功能】求两个集合的并集
【时间复杂度】O(n)
(11) sdiffstore newSet set1 set2
(12) sintersotre newSet set1 set2
(13) sunionstore newSet set1 set2

=============================================

#### zset
1. 结构
 key    -->  score |  value
 ------------------------------
              1    | narlinen
ranking -->   2    | yuki
		      5    | mafuyu
			  12   | miuna
2. 特点
元素不重复，有序
3. API
(1) zadd
【格式】zadd key score element [可以是多对]
【功能】添加score-element
【时间复杂度】O(logN)
(2) zrem
【格式】zrem key element
【功能】删除集合中指定索引的element
【时间复杂度】O(1)
(3) zscore
【格式】zcore key element
【功能】获取集合中element的score
【时间复杂度】O(n)
(4) zincrby
【格式】zincrby key increScore element
【功能】将集合中element的score增加increScore
【注意】当increScore为负数时，实现减少的功能
【时间复杂度】O(1)
(5) zcard
【格式】zcard key
【功能】返回集合中元素的个数
【时间复杂度】O(1)
(6) zrank
【格式】zrank key element
【功能】获取集合中指定元素的排名
【时间复杂度】O(1)
(7) zrange
【格式】zrange key start end [withscores]
【功能】获取集合中排行从start到end的元素
【注意】以score从小到达进行排序；加上withscores会返回element的score
【时间复杂度】O(log(n)+m)
(8) zrangebyscore
【格式】zrangebyscore key minScore maxScore
【功能】获取集合中score位于[minScore,maxScore]之间的元素
【时间复杂度】O(log(n)+m)
(9) zcount
【格式】zcount key minScore maxScore
【功能】获取集合中score处于[minScore,maxScore]之间的element个数
【时间复杂度】O(log(n)+m)
(10) zremrangebyrank
【格式】zremrangebyrank key start end
【功能】删除指定排名内的元素
【时间复杂度】O(log(n)+m)
(11) zremrangebyscore
【格式】zremrangebyscore key minScore maxScore
【功能】删除指定分数内的元素
【时间复杂度】O(log(n)+m)
(12) zrevrank
(13) zrevrange
(14) zrevrangebyscore
(15) zinterstore
(16) zunionstore

===================================================================================

#### Jedis
1. 什么是Jedis
Java语言操作redis的工具

2. 获取Jedis.jar
​```
http://mvnrepository.com/artifact/redis.clients/jedis/2.9.0
​```

3. Jedis直连
​```
Jedis jedis = new Jedis("127.0.0.1",6379);
jedis.set("name","narlinen");
String name = jedis.get("name");
jedis.close();
​```

4. Jedis连接池
​```
JedisPoolConfig poolConfig = new GenericObjectPoolConfig();
JedisPool jedisPool = new JedisPool(poolConfig,"127.0.0.1",6379);
jedis = jedisPool.getJedis();
jedis.close();
jedisPool.close();
​```

=====================================================================================

#### 慢查询

=====================================================================================

#### pipeline
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

#### 发布订阅
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

#### bitmap
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

#### HyperLogLog
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

#### 持久化
1. 持久化概念
将redis存储在内存中的数据存储在硬盘中
2. 持久化方式
RDB：存储键值对【快照】
AOF：存储操作命令【日志】
3. RDB和AOF对比
|命令|RDB|AOF|
|---|---|---|
|启动优先级|低|高|
|体积|小|大|
|恢复速度|快|慢|
|数据安全性|丢数据|根据策略|
|轻重|重|轻|

#### RDB方式
1. 三种触发方式
save触发方式
(1) 使用
redis-cli> save
(2) 原理
在原线程上创建RDB文件
(3) 运行图
        save          create
client -----> redis -----------> RDB
(4) 特点
阻塞式

bgsave触发方式
(1) 使用
redis-cli> bgsave
(2) 原理
先fork出子进程，然后在该子进程中
(3) 运行图
        1.bgsave           正常服务
client -----------> redis ----------> clients
                  ||
        2.fork()  || 4.bgsave successfully
		    	  ||
			      ||    3.create	
			     redis ------------> RDB
(4) 特点
fork进程是阻塞的
创建RDB是非阻塞的
				
auto触发方式
(1) 使用
根据配置文件自动调用bgsave
(2) 原理
同bgsave
(3) 运行图
同bgsave
(4) 特点
配置redis.conf
save 900 1     #900秒时间，至少有一条数据更新，则保存到数据文件中
save 300 10    #300秒时间，至少有10条数据更新，则保存到数据文件中
save 60 10000  #60秒时间，至少有10000条数据更新，则保存到数据文件中

2. save和bgsave对比
|命令|save|bgsave|
|---|---|---|
|IO类型|同步|异步|
|是否阻塞|是|是(阻塞发生在fork上)|
|复杂度|O(n)|O(n)|
|优点|不会消耗额外内存|不阻塞客户端命令|
|缺点|阻塞客户端命令|需要fork，消耗内存|

3. RDB的配置
dbfilename dump.rdb             #配置RDB文件的名字
rdbcompression yes              #是否压缩RDB文件【推荐yes】
rdbchecksum yes                 #是否开启检验和【推荐yes】
stop-writes-on-bgsave-error yes #是否在出错时停止【推荐yes】

4. RDB缺点
①  耗时
②  可能会丢数据

#### AOF方式
1. 三种触发方式
|触发方式|always|eversec|no|
|特点|每次命令都记录|每秒记录|OS来决定记录|
|优点|不丢失数据|每秒1次|不用管|
|缺点|IO开销较大|可能丢1s数据|不可控|

2. AOF配置
appendonly yes                #是否启动AOF
appendfilename "${port}.aof"  #AOF文件名
appendfsync everysec          #设置AOF触发方式
dir ./                        #设置AOF文件存放位置
no-appendfsync-on-rewrite yes #重写时是否不继续写入新命令【推荐yes】

3. AOF重写
合并命令，删除重复命令，删除过期命令
从而达到减少AOF文件大小

4. AOF重写的配置
auto-aof-rewrite-min-size   AOF文件重写需要的尺寸
auto-aof-rewrite-percentage AOF文件增长率

aof_current_size AOF当前尺寸
aof_base_size AOF上次启动和重写的尺寸

自动触发时机
aof_current_size > auto-aof-rewrite-min-size
aof_current_size - aof_base_size/aof_base_size > auto-aof-rewrite-percentage

=============================================================================

#### 主从复制
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

#### 主从复制实现故障转移
图片

==========================================

#### redis-sentinel
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
# sentinel本身是一个redis，所以有redis的基础配置
daemonize yes        # 使用守护线程
port 23679           # 占用端口号
dir "~/data"         # 工作目录
logfile "26379.conf" # 日志名称

# sentinel特有的配置

## sentinel监控的master的名字叫做mymaster,地址为127.0.0.1:6379
## 2代表当sentinel集群中有两个sentinel认为master宕机时，才真正认为该master不可用
sentinel monitor mymaster 127.0.0.1 6379 2

## sentinel会向master发送心跳PING来确认master是否存活
## 如果master在“down-after-milliseconds”内不回应PONG,或者是回复了一个错误消息
## 那么这个sentinel会认为这个master已经不可用了
sentinel down-after-milliseconds mymaster 6000

## 
sentinel failover-timeout mymaster 18000

## 在发生主备切换时，这个选项指定了最多可以有多少个slave同时对新的master进行同步
## 这个数字越小，完成切换所需的时间就越长，但是如果这个数字越大，就意味着越多的slave因为replication而不可用
## 可以通过将这个值设为 1 来保证每次只有一个slave处于不能处理命令请求的状态。
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

#### redis-cli 使用
redis-cli -p port #连接到指定端口的redis中
redis-cli -p port shutdown #关闭指定端口的redis-server
redis-cli -p port info replication #查看指定端口的redis信息
redis-cli> info #查看当前连接的redis-server的信息
```