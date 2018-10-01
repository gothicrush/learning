## 配置

### 配置文件位置
```
sudo vim /etc/my.cnf
```

### 配置介绍
```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
socket=/var/lib/mysql/mysql.sock

[mysqld]
skip-name-resolve
#设置3306端口
port = 3306 
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装目录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql/data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
lower_case_table_name=1
max_allowed_packet=16M
```



## 存储引擎

### 查看存储引擎介绍

```mysql
SHOW ENGINES;
```

### 查看默认存储引擎与当前存储引擎

```mysql
SHOW VARIABLES LIKE '%storage_engine%';
```

### InnoDB和MyISAM对比

| 对比项 | MyISAM                                                 | InnoDB                                     |
| ------ | ------------------------------------------------------ | ------------------------------------------ |
| 主外键 | 不支持                                                 | 支持                                       |
| 事务   | 不支持                                                 | 支持                                       |
| 行表锁 | 表锁；即使操作一条记录也会锁住整张表，不适合高并发操作 | 行锁；操作时只会锁住某一行，适合高并发操作 |
| 缓存   | 只缓存索引，不缓存真实数据，内存要求小                 | 索引和真实数据都可以缓存，内存要求高       |
| 表空间 | 小                                                     | 大                                         |
| 关注点 | 性能                                                   | 事务                                       |

