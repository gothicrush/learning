### redis是什么

- 键值对存储系统

### redis特点

* 开源免费

* 支持多种编程语言

* 高性能
  * 数据存储在内存中
  * 用C语言编写
  * 单线程模型
    * 避免线程切换和竞争带来的性能消耗
    * 一次只执行一条命令

* 支持持久化
  * 支持RDB和AOF两种持久化方式

* 支持多种数据结构
  * string, list, hash, set, sorted set
  * bitmap, hyperloglog, geo

* 功能丰富
  * 慢查询
  * pipeline
  * 发布订阅

* 简单
  * 仅仅用C语言，5w行
  * 不依赖第三方库
  * 单线程模型

* 具有主从复制，高可用，分布式功能
  * redis-sentinel
  * redis-cluster

  

### redis典型使用场景

- 缓存系统
- 计数器
- 排行榜
- 消息队列系统
- 实时系统

### redis下载与安装

| 操作 | 说明                             |
| ---- | -------------------------------- |
| 下载 | https://redis.io/download        |
| 解压 | tar -zxvf redis.tar.gz           |
| 安装 | cd redis && make && make install |

### redis/src目录文件

| 文件名           | 功能              |
| ---------------- | ----------------- |
| redis-server     | redis服务器       |
| redis-cli        | redis客户端       |
| redis-benchmark  | redis性能测试工具 |
| redis-check-aof  | AOF文件修复工具   |
| redis-check-dump | RDB文件修复工具   |
| redis-sentinel   | sentinel服务器    |

### 启动与关闭redis服务端

| 操作         | 说明                                       |
| ------------ | ------------------------------------------ |
| 最简启动     | ${redis}/src/redis-server                  |
| 配置文件启动 | ${redis}/src/redis-server config_file.conf |
| 动态参数启动 | ${redis}/src/redis-server --port 6380      |
| 检查是否启动 | ps -ef \| grep redis                       |
| 关闭服务端   | ${redis}/src/redis-cli -p 6380 shutdown    |

### 启动与关闭redis客户端

| 操作 | 说明                                     |
| ---- | ---------------------------------------- |
| 启动 | ${redis}/src/redis-cli -h {ip} -p {port} |
| 关闭 | exit                                     |

### redis配置文件

| 参数      | 说明                   | 默认值 |
| --------- | ---------------------- | ------ |
| daemonize | 是否以守护进程方式启动 | on     |
| port      | redis占用端口          | 6379   |
| logfile   | redis系统日志          |        |
| dir       | redis工作目录          |        |
