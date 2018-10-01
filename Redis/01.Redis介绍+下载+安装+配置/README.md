### redis是什么

- 开源|免费|高性能|单线程的键值对缓存系统

### redis典型使用场景

- 缓存系统
- 计数器
- 排行榜
- 消息队列系统
- 实时系统

### redis下载与安装

| 操作 | 说明                               |
| ---- | ---------------------------------- |
| 下载 | http://download.redis.io/releases/ |
| 解压 | tar -zxvf redis.tar.gz             |
| 安装 | cd redis && make && make install   |

### 启动与关闭redis服务端

| 操作         | 说明                                       |
| ------------ | ------------------------------------------ |
| 最简启动     | ${redis}/src/redis-server                  |
| 配置文件启动 | ${redis}/src/redis-server config_file.conf |
| 动态参数启动 | ${redis}/src/redis-server --port 6380      |
| 检查是否启动 | ps -ef \| grep redis                       |
| 关闭服务端   | ${redis}/src/redis-cli shutdown            |

### 启动与关闭redis客户端

| 操作 | 说明                   |
| ---- | ---------------------- |
| 启动 | ${redis}/src/redis-cli |
| 关闭 | exit                   |

### redis配置文件

| 参数      | 说明                   | 默认值 |
| --------- | ---------------------- | ------ |
| daemonize | 是否以守护进程方式启动 | on     |
| port      | redis占用端口          | 6379   |
| logfile   | redis系统日志          |        |
| dir       | redis工作目录          |        |

### 