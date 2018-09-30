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
ps -ef | grep redis-
```

* 使用redis-cli连接redis-server，并退出
```
src/redis-cli -p 6666
redis> exit
```

* 关闭redis-server
```
src/redis-cli -p shutdown
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

* 在不存在times的前提下，设置times-0
```
setnx times 0
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

* 先获取time的值，再设置为10
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

==============================================

* 设置hash-person1：name narlinen
```hset person1 name narlinen```

* 批量设置hash-person1：age 20，gender male，color red
```hmset person1 age 20 gender male color red```

* 确保times不存在时，设置times 10
```hsetnx person1 times 10```

* 使times自增1，查看
* 使times自减1，查看
* 使times增加2，查看
* 使times减少2，查看
* 使times增加1.5，查看
* 使times减少1.5，查看
```hincrby person1 times 1```
```hincrby person1 times -1```
```hincrby person1 times 2```
```hincrby person1 times -2```
```hincrbyfloat person1 times -1.5```
```hincrbyfloat person1 times -1.5```

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
```hvals person1```

* 删除person1中的name,age,gender,color,times
```hdel person1 name age gender color times```

* 删除person1
```del person1```

====================================================

### list类型

* 以左插入创建列表lista，内容为a->b->c->d->e->f->g->h，并查看
```
lpush lista h g f e d c b a
lrange lista 0 -1
j```

* 以右插入创建列表listb，内容为a->b->c->d->e->f->g->h，并查看
```rpush listb a b c d e f g h```
```lrange listb 0 -1```

* 在lista的f元素前插入before_f，并查看
```linsert lista before b before_b```

* 在listb的f元素后插入after_f，并查看
```linsert listb after b after_b```

* 弹出lista的a元素，并查看
```lpop lista```

* 弹出listb的d元素，并查看
```rpop listh```

* 阻塞3秒后弹出lista的b元素，并查看
```blpop lista 3000```

* 阻塞3秒后弹出listb的g元素，并查看
```brpop listb 3000```

* 查看lista，再修改其第二个元素为new2，并查看
```lrange lista 0 -1```<br>
```lset lista 1 new2```<br>
```lrange lista 0 -1```

* 获取lista中索引为1的元素
```lindex lista 1```

* 获取lista的长度
```llen lista```

* 删除lista的before_f，并查看
```lrem lista 1 before_b```

* 删除listb的after_f，并查看
```lrem listb -1 after_b```

* 查看listb，然后删除listb的头尾两个元素，并查看
```lrange listb 0 -1```<br>
```ltrim listb 1 -2```<br>
```lrange listb 0 -1```

* 删除lista和listb
```del lista listb```

============================================================

* 创建集合seta，内容为a,b,c,d
```sadd seta a b c d```

* 创建集合setb，内容为d,e,f,g
```sadd setb d e f g```

* 获取集合seta的元素个数
```scard seta```

* 判断e是否为seta和setb的成员
```sismember seta e```<br>
```sismember setb e```

* 随机选出seta的一个元素
```srandmember seta```

* 获取seta中所有的元素
```smembers seta```

* 查看seta和setb的交集，然后保存为setabinter
```sinter seta setb```<br>
```sinterstore setabinter seta setb```

* 查看seta和setb的并集，然后保存为setabunion
```sunion seta setb```<br>
```sunionstore setabunion seta setb```

* 查看seta和setbd的差集，然后保存为setabdiff
```sdiff seta setb```<br>
```sdiffstore setabdiff seta setb```

* 删除setb的d元素
```srem setb d```

* 删除seta和setb
```del seta setb```

================================================================

* 创建zset myzset，内容为1 narlinen 2 mafuyu 3 miuna 4 random
```zadd myzset 1 narlinen 2 mafuyu 3 miuna 4 random```

* 获取myzset的个数
```zcard myzset```

* 删除random
```zrem myzset random```

* 获取narlinen的score
```zscore myzset narlinen```

【未完待续】

=================================================================

* 设置redis-7654中键值对name-narlinen，并用rdb备份为7654.rdb，修改name为other
```set name narlinen```
```bgsave```
```set name other```

* 退出redis-7654，重新打开redis-7654，查看name的值是不是narlinen
```get name```

=================================================================

* 设置redis-4567中键值对age-20，并用aof每命令备份7654.aof，修改age为21
```set age 20```

* 退出redis-4567，重新打开redis-4567，查看age是不是20
```get age 20```