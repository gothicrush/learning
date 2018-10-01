### 持久化

- 持久化概念
  - 将redis存储在内存中的数据存储在硬盘中
  - 计算机不会因断电而造成数据丢失
- 持久化方式

| 方式 | 说明                 |
| ---- | -------------------- |
| RDB  | 存储键值对【快照】   |
| AOF  | 存储操作命令【日志】 |

- RDB和AOF对比

| 命令       | RDB    | AOF      |
| ---------- | ------ | -------- |
| 启动优先级 | 低     | 高       |
| 体积       | 小     | 大       |
| 恢复速度   | 快     | 慢       |
| 数据安全性 | 丢数据 | 根据策略 |
| 轻重       | 重     | 轻       |

### RDB方式

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

1. save和bgsave对比

| 命令     | save             | bgsave               |
| -------- | ---------------- | -------------------- |
| IO类型   | 同步             | 异步                 |
| 是否阻塞 | 是               | 是(阻塞发生在fork上) |
| 复杂度   | O(n)             | O(n)                 |
| 优点     | 不会消耗额外内存 | 不阻塞客户端命令     |
| 缺点     | 阻塞客户端命令   | 需要fork，消耗内存   |

1. RDB的配置
   dbfilename dump.rdb             #配置RDB文件的名字
   rdbcompression yes              #是否压缩RDB文件【推荐yes】
   rdbchecksum yes                 #是否开启检验和【推荐yes】
   stop-writes-on-bgsave-error yes #是否在出错时停止【推荐yes】
2. RDB缺点
   ①  耗时
   ②  可能会丢数据

### AOF方式

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