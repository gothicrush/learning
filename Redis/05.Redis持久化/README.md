### 数据持久化

- 数据持久化概念
  - 将redis存储在内存中的数据存储在硬盘中
  - 计算机不会因断电而造成数据丢失
- 数据持久化方式

| 方式 | 说明                   |
| ---- | ---------------------- |
| RDB  | 存储键值对【记录快照】 |
| AOF  | 存储操作命令【写日志】 |



### RDB方式

#### RDB方式是什么

* 备份：将内存中的数据保存在磁盘中的一个文件中
* 恢复：根据磁盘中的文件，将数据写入内存中

#### RDB三种触发方式

* save触发方式[同步]

  *  使用：redis-cli> save

  * 原理：在原线程上创建RDB文件

  * 运行图

    ```bash
                 save                  create 
    client ---------------> redis ---------------> RDB
    ```

  * 特点：阻塞式

* bgsave触发方式[异步]

  * 使用：redis-cli> bgsave

  * 原理：先fork出子进程，然后在该子进程中

  * 运行图

    ```bash
               1.bgsave                            正常服务
    client  -------------------------> redis ---------------------> clients
                  |                      ^  
                  |                      |
              2.fork()                   | 4.bgsave successfully
                  |                      | 
                  v        3.create      |
             redis子进程 ------------> RDB文件
    ```

  * 特点：fork进程的过程是阻塞的；子进程创建RDB是非阻塞的

* 配置触发方式

  * 使用：根据配置文件自动调用bgsave

  * 原理：与bgsave方式相同

  * 运行图：与bgsave方式相同

  * 配置：

    ```bash
    # 配置redis.conf
    save 900 1     #900秒时间，至少有一条数据更新，则保存到数据文件中
    save 300 10    #300秒时间，至少有10条数据更新，则保存到数据文件中
    save 60 10000  #60秒时间，至少有10000条数据更新，则保存到数据文件中
    ```

#### save和bgsave对比

| 命令     | save             | bgsave               |
| -------- | ---------------- | -------------------- |
| IO类型   | 同步             | 异步                 |
| 是否阻塞 | 是               | 是(阻塞发生在fork上) |
| 复杂度   | O(n)             | O(n)                 |
| 优点     | 不会消耗额外内存 | 不阻塞客户端命令     |
| 缺点     | 阻塞客户端命令   | 需要fork，消耗内存   |

#### RDB的相关配置

| 配置项                              | 配置说明                    |
| ----------------------------------- | --------------------------- |
| dbfilename dump.rdb                 | 配置RDB文件的名字           |
| rdbcompression yes\|no              | 是否压缩RDB文件【推荐yes】  |
| rdbchecksum yes\|no                 | 是否开启检验和【推荐yes】   |
| stop-writes-on-bgsave-error yes\|no | 是否在出错时停止【推荐yes】 |

#### RDB存在的严重问题

* 可能会丢数据

#### RDB三种自动触发时机

* 全量复制
* debug reload
* shutdown



### AOF方式

#### AOF方式是什么

* 一种将内存中数据保存到本地硬盘中的持久化方式
* AOF记录的不是数据本身，而是记录产生数据的命令

#### AOF三种触发方式

| 触发方式 | 特点                           | 优点             | 缺点              |
| -------- | ------------------------------ | ---------------- | ----------------- |
| always   | 每条记录都会将缓冲区写入硬盘   | 基本不会丢失数据 | IO开销大          |
| everysec | 每秒都会将缓冲区写入硬盘       | 每秒1次          | 可能会丢失1秒数据 |
| no       | OS自己决定什么将缓冲区写入硬盘 | 不用管理         | 不能管理          |

#### AOF重写

* 功能：通过合并命令，删除重复命令，删除过期命令，从而达到减少AOF文件大小 

* 使用：

  * redis-cli> bgrewriteaof

    ```bash
            bgrewriteaof
    client <-------------> redis
                  ok         |
                             |
                             v       AOF重写
                        redis子进程 -----------> AOF重写文件
    ```

  * 通过AOF重写配置

#### AOF配置

| 配置项                             | 配置说明                               |
| ---------------------------------- | -------------------------------------- |
| appendonly   yes\|no               | 是否开启AOF                            |
| appendfilename  xxx.aof            | AOF文件名                              |
| appendfsync always\|everysec\|no   | AOF触发方式【默认everysec】            |
| dir ./                             | AOF文件存放目录                        |
| no-appendfsync-on-rewrite  yes\|no | 重写时是否不继续写入新命令 【推荐yes】 |
| auto-aof-rewrite-min-size          | AOF文件重写需要的尺寸                  |
| auto-aof-rewrite-percentage        | AOF文件重写增长率                      |

#### AOF统计与触发时机

* 统计

| 统计名           | 说明                    |
| ---------------- | ----------------------- |
| aof_current_size | AOF当前尺寸             |
| aof_base_size    | AOF上次启动和重写的尺寸 |

* 触发时机
  * aof_current_size > auto-aof-rewrite-min-size
  * aof_current_size - aof_base_size/aof_base_size > auto-aof-rewrite-percentage



### RDB和AOF对比

| 命令       | RDB    | AOF      |
| ---------- | ------ | -------- |
| 启动优先级 | 低     | 高       |
| 体积       | 小     | 大       |
| 恢复速度   | 快     | 慢       |
| 数据安全性 | 丢数据 | 根据策略 |
| 轻重       | 重     | 轻       |
