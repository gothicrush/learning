### 下载|安装

* 下载redis

  ```bash
  https://redis.io/download
  ```

* 安装路径为/usr/local/redis

  ```bash
  sudo mv redis /usr/local/redis
  cd /usr/local/redis
  sudo make && sudo make install
  ```

### 配置|启动|连接

* 配置新的redis-server，其端口号为6789，以守护线程，工作目录为"/home/narlinen"，日志文件名为"log-6789.log"
```
vim conf/redis-6789.conf
daemonize yes
port 6789
dir "/home/narlinen"
logfile "log-6789.log"
```

* 用三种方式启动redis-server
```
src/redis-server
src/redis-server --port 6666
src/redis-server conf/redis-6666.conf
```

* 查看是否成功启动redis-server
```
ps -ef | grep redis
```

* 使用redis-cli连接redis-server，测试是否连接成功，最后并退出
```
src/redis-cli -h 127.0.0.1 -p 6666
127.0.0.1:6666> ping
127.0.0.1:6666> exit
```

* 关闭redis-server
```
src/redis-cli -p 6666 shutdown
```

### 通用命令

* 设置 hello-world，hehe-haha，hen-cow

  ```bash
  mset hello world hehe haha hen cow
  ```

* 查询redis中he*的键

  ```bash
  keys he*
  ```

* 查询hehe的类型

  ```bash
  type hehe
  ```

* 查询数据库中key的数量

  ```bash
  dbsize
  ```

* 为hehe设置过其时间200秒

  ```bash
  expire hehe 200
  ```

* 查看hehe剩余过期时间

  ```bash
  ttl hehe
  ```

* 清除hehe过期时间

  ```bash
  persist hehe
  ```

* 为hen设置过其时间60秒

  ```bash
  expire hen 60
  ```

* 查看hen是否存在

  ```bash
  exists hen
  ```

* 60秒后再次查看hen是否存在

  ```bash
  exists hen
  ```

* 把hello，hehe，hen都删除

  ```bash
  del hello hehe hen
  ```

* 查询数据库中key的数量

  ```bash
  dbsize
  ```

### string类型

* 设置name-narlinen
```
set name narlinen
```

* 批量设置age-20,gender-male,color-red
```
mset age 20 gender male color red
```

* 获取gender前3个字母
```bash
getrange gender 0 2
```

* 将gender第2个字母设置为z，并查看

```bash
setrange gender 1 z
get gender
```

* 在不存在times的前提下，设置times-0
```
setnx times 0
```

* 在存在times的前提下，设置times-11
```bash
set times 11 xx
```

* 获取name的值
```
get name
```

* 批量获取age,gender,color的值
```
mget age gender color
```

* 使times自增1，使times自减1
```
incr times
decr times
```

* 使times增加2，使times减少2
```
incrby times 2
decrby times 2
```
*  使times增加1.5，使times减少1.5
```
incrbyfloat times 1.5
incrbyfloat times -1.5
```

* 先获取times的值，再设置为10
```
getset times 10
```

* 获取name的长度
```
strlen name
```

* 追加name后面"-abc"
```
append name -abc
```

* 删除name,age,gender,color,times
```
del name age gender color times
```

### hash类型

* 设置hash : person1：name narlinen
```
hset person1 name narlinen
```

* 批量设置hash : person1：age 20，gender male，color red
```
hmset person1 age 20 gender male color red
```

* 确保person1中times不存在时，设置times 10
```
hsetnx person1 times 10
```

* 使times自增1，使times自减1
```
hincrby person1 times 1
hincrby person1 times -1
```

* 使times增加2，使times减少2
```
hincrby person1 times 2
hincrby person1 times -2
```

* 使times增加1.5，使times减少1.5
```
hincrbyfloat person1 times -1.5
hincrbyfloat person1 times -1.5
```

* 获取hash-person1：name
```
hget person1 name
```

* 批量获取hash-person1：age,gender,color
```
hmget person1 age gender color
```

* 获取hash-person1的键个数
```
hlen person1
```

* 判断hash-person1中是否存在键name和book
```
hexists person1 name
hexists person1 book
```

* 获取hash-person1中所有键值对
```
hgetall person1
```

* 获取hash-person1中所有键
```
hkeys person1
```

* 获取hash-person1中所有值
```
hvals person1
```

* 删除person1中的name,age,gender,color,times
```
hdel person1 name age gender color times
```

* 删除person1
```
del person1
```

### list类型

* 以左插入创建列表lista，内容为a->b->c->d->e->f->g->h，并查看
```
lpush lista h g f e d c b a
lrange lista 0 -1
```

* 以右插入创建列表listb，内容为a->b->c->d->e->f->g->h，并查看
```
rpush listb a b c d e f g h
lrange listb 0 -1
```

* 在lista的f元素前插入before_f，并查看
```
linsert lista before b before_b
```

* 在listb的f元素后插入after_f，并查看
```
linsert listb after b after_b
```

* 弹出lista的最左元素，并查看
```
lpop lista
```

* 弹出listb的最右元素，并查看
```
rpop listb
```

* 阻塞3秒后弹出lista的b最左元素，并查看
```
blpop lista 3000
```

* 阻塞3秒后弹出listb的最右元素，并查看
```
brpop listb 3000
```

* 查看lista，再修改其第二个元素为new2，并查看
```
lrange lista 0 -1
lset lista 1 new2
lrange lista 0 -1
```

* 获取lista中索引为1的元素
```
lindex lista 1
```

* 获取lista的长度
```
llen lista
```

* 删除lista的before_f，并查看
```
lrem lista 1 before_f
```

* 删除listb的after_f，并查看
```
lrem listb -1 after_f
```

* 查看listb，然后删除listb的头尾两个元素，并查看
```
lrange listb 0 -1
ltrim listb 1 -2
lrange listb 0 -1
```

* 删除lista和listb
```
del lista listb
```

### set类型

* 创建集合seta，内容为a,b,c,d
```
sadd seta a b c d
```

* 创建集合setb，内容为d,e,f,g
```
sadd setb d e f g
```

* 获取集合seta的元素个数
```
scard seta
```

* 判断e是否为seta和setb的成员
```
sismember seta e
sismember setb e
```

* 随机选出seta的一个元素
```
srandmember seta
```

* 获取seta中所有的元素
```
smembers seta
```

* 查看seta和setb的交集，然后保存为setabinter
```
sinter seta setb
sinterstore setabinter seta setb
```

* 查看seta和setb的并集，然后保存为setabunion
```
sunion seta setb
sunionstore setabunion seta setb
```

* 查看seta和setbd的差集，然后保存为setabdiff
```
sdiff seta setb
sdiffstore setabdiff seta setb
```

* 删除setb的d元素
```
srem setb d
```

* 删除seta和setb
```
del seta setb
```

### zset类型

* 创建zset myzset，内容为1 narlinen 2 mafuyu 3 miuna 4 random
```
zadd myzset 1 narlinen 2 mafuyu 3 miuna 4 random
```

* 获取myzset的个数
```
zcard myzset
```

* 删除random
```
zrem myzset random
```

* 获取narlinen的score
```
zscore myzset narlinen
```

### RDB

* 设置redis-7654中rdb文件名为dump-7654.rdb，开启rdb压缩，开启校验和，设置出错时停止
```bash
rdbfilename dump-7654.rdb
rdbcompression yes
rdbchecksum yes
stop-writes-on-bgsave-error yes
```

* 连接redis-7654，设置键值对name-narlinen，并save，然后设置name为other
```bash
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> set name narlinen
127.0.0.1:7654> save
127.0.0.1:7654> set name other
```

* 关闭redis-7654，重新打开redis-7654，查看name的值是不是narlinen
```
127.0.0.1:7654> exit
>> redis-cli -h 127.0.0.1 -p 7654 shutdown
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> get name
```

* 删除dump-7654.rdb，重连查看name是否存在
```bash
127.0.0.1:7654> exit
>> redis-cli -h 127.0.0.1 -p 7654 shutdown
>> rm -rf dump-7654.rdb
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> get name
```

* 设置键值对age-20，并bgsave，然后设置age为30
```
127.0.0.1:7654> set age 20
127.0.0.1:7654> bgsave
127.0.0.1:7654> set age 30
```

* 退出redis-4567，重新打开redis-4567，查看age是不是20
```
127.0.0.1:7654> exit
>> redis-cli -h 127.0.0.1 -p 7654 shutdown
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> get age
```

* 删除dump-7654.rdb，设置配置每60秒有1条命令就进行rbd持久化
```bash
127.0.0.1:7654> exit
>> redis-cli -h 127.0.0.1 -p 7654 shutdown
>> rm -rf dump-7654.rdb
>> vim 7654.conf
>>>> save 60 1
```

* 设置键值对 gender-male，退出重连，查看gender是不是male
```bash
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> set gender male
127.0.0.1:7654> exit
>> redis-cli -h 127.0.0.1 -p 7654 shutdown
>> redis-server 7654.conf
>> redis-cli -h 127.0.0.1 -p 7654
127.0.0.1:7654> get gender
```

### AOF

* 开启AOF，AOF文件名为9999.aof，模式为每条命令都记录，重写时不追加新命令，自动重写尺寸为64MB，自动重写增长率为100

  ```bash
  appendonly yes
  appendfilename 9999.aof
  appendfsync always
  no-appendfsync-on-rewrite yes
  auto-aof-rewrite-min-size 64mb
  auto-aof-rewrite-percentage 100
  ```

* 开启redis-9999，连接redis-9999，设置键值对name-narlinen，退出redis，关闭redis-9999

  ```bash
  >> redis-server 9999.conf
  >> redis-cli -h 127.0.0.1 -p 9999
  127.0.0.1:9999> set name narlinen
  127.0.0.1:9999> exit
  >> redis-cli -h 127.0.0.1 -p 9999 shutdown
  ```

* 重新开启redis-9999，查看name是否存在

  ```bash
  >> redis-server 9999.conf
  >> redis-cli -h 127.0.0.1 -p 9999
  127.0.0.1:9999> get name
  ```

  

### 慢查询

* 配置慢查询队列长度为100

  ```bash
  slowlog-max-len 100
  ```

* 配置命令时间超过10毫秒算慢命令

  ```bash
  slowlog-log-slower-than 10000
  ```

* 启动redis-server，连接到该server，然后修改慢查询队列长度为200，超时时间为0

  ```bash
  redis-server configurations/6666.conf
  redis-cli -h 127.0.0.1 -p 6666
  127.0.0.1:6666> config set slowlog-max-len 200
  127.0.0.1:6666> config set slowlog-log-slower-than 0
  ```

* 写入键值对 hello-world，today-tomorrow，读取hello的值

  ```bash
  127.0.0.1:6666> set hello world
  127.0.0.1:6666> set today tomorrow
  127.0.0.1:6666> get hello
  ```

* 查询此时慢查询队列中元素个数

  ```bash
  127.0.0.1:6666> slowlog len
  ```

* 查询每个慢查询元素的信息

  ```bash
  127.0.0.1:6666> slowlog get[0]
  127.0.0.1:6666> slowlog get[1]
  127.0.0.1:6666> slowlog get[2]
  ```

* 清空慢查询队列，再次查看队列中元素个数

  ```bash
  127.0.0.1:6666> slowlog reset
  127.0.0.1:6666> slowlog len
  ```



### 发布订阅

* 创建3个client，A,B,C，其中A订阅voice:of:redis，B订阅voice:of:nosql，C两个都订阅

  ```bash
  127.0.0.1:6666(client-A)> subscribe voice:of:redis
  127.0.0.1:6666(client-B)> subscribe voice:of:nosql
  127.0.0.1:6666(client-C)> subscribe voice:of:redis
  127.0.0.1:6666(client-C)> subscribe voice:of:nosql
  ```

* voice:of:redis发布信息"I'm voice of redis"，voice:of:nosql发布信息"I'm voice of nosql"，观察A,B,C的状态

  ```&gt;bash
  127.0.0.1:6666(D)> publish voice:of:redis "I'm voice of redis"
  127.0.0.1:6666(D)> publish voice:of:nosql "I'm voice of nosql"
  ```

* C取消订阅voice:of:nosql，voice:of:redis和voice:of:nosql都再次发布刚刚的信息，观察C的状态

  ```bash
  127.0.0.1:6666(client-C)> unsubscribe voice:of:nosql
  127.0.0.1:6666(client-1)> publish voice:of:redis
  127.0.0.1:6666(client-2)> publish voice:of:nosql
  ```

* 查询有订阅者的频道，并查看各自有多少人订阅

  ```bash
  127.0.0.1:6666(D)> pubsub channels
  127.0.0.1:6666(D)> pubsub numsub voice:of:redis
  127.0.0.1:6666(D)> pubsub numsub voice:of:nosql
  ```

  

### 主从复制

* 开启redis-6666，redis-7777，redis-8888，redis-9999

  ```bash
  >> redis-server 6666.conf
  >> redis-server 7777.conf
  >> redis-server 8888.conf
  >> redis-server 9999.conf
  ```

* 分别设置 port-6666|66-66，port-7777|77-77，port-8888|88-88，port-9999|99-99

  ```bash
  127.0.0.1:6666> set port 6666
  127.0.0.1:6666> set 66 66 
  127.0.0.1:7777> set port 7777
  127.0.0.1:7777> set 77 77
  127.0.0.1:8888> set port 8888
  127.0.0.1:8888> set 88 88
  127.0.0.1:9999> set port 9999
  127.0.0.1:9999> set 99 99
  ```

* 通过slaveof命令，将6666，7777，8888成为9999的从节点

  ```bash
  127.0.0.1:6666> slaveof 127.0.0.1 9999
  127.0.0.1:7777> slaveof 127.0.0.1 9999
  127.0.0.1:8888> slaveof 127.0.0.1 9999
  ```

* 分别查看66，77，88是否还存在；查看是否有99；查看port

  ```bash
  127.0.0.1:6666> get 66
  127.0.0.1:6666> get 99
  127.0.0.1:6666> get port
  127.0.0.1:7777> get 77
  127.0.0.1:7777> get 99
  127.0.0.1:7777> get port
  127.0.0.1:8888> get 88
  127.0.0.1:8888> get 99
  127.0.0.1:8888> get port
  ```

* 在redis-9999添加name-narlinen

  ```bash
  127.0.0.1:9999> set name narlinen
  ```

* 查看redis-6666，redis-7777，redis-8888中是否存在name

  ```bash
  127.0.0.1:6666> get name
  127.0.0.1:7777> get name
  127.0.0.1:8888> get name
  ```

* 在redis-9999中删除name

  ```bash
  127.0.0.1:9999> del name
  ```

* 查看redis-6666，redis-7777，redis-8888中是否存在name

  ```bash
  127.0.0.1:6666> get name
  127.0.0.1:7777> get name
  127.0.0.1:8888> get name
  ```

* 令 redis-6666取消跟随redis-9999

  ```bash
  127.0.0.1:6666> slaveof no one
  ```

* 在redis-9999中加入键值对 age-20，分别在redis-6666，redis-7777，redis-8888中查看age

  ```
  127.0.0.1:9999> set age 20
  127.0.0.1:6666> get age
  127.0.0.1:7777> get age
  127.0.0.1:8888> get age
  ```

* 通过slaveof ip port 的方式设置redis-6666，redis-7777，redis-8888为redis-9999的从节点

  ```bash
  slaveof 9999 127.0.0.1 # 6666.conf
  slaveof 9999 127.0.0.1 # 7777.conf
  slaveof 9999 127.0.0.1 # 8888.conf
  ```

* 在redis-9999添加name-narlinen

  ```bash
  127.0.0.1:9999> set name narlinen
  ```

* 查看redis-6666，redis-7777，redis-8888中是否存在name

  ```bash
  127.0.0.1:6666> get name
  127.0.0.1:7777> get name
  127.0.0.1:8888> get name
  ```

* 在redis-9999中删除name

  ```bash
  127.0.0.1:9999> del name
  ```

* 查看redis-6666，redis-7777，redis-8888中是否存在name

  ```bash
  127.0.0.1:6666> get name
  127.0.0.1:7777> get name
  127.0.0.1:8888> get name
  ```