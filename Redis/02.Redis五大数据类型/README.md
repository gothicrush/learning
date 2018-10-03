### redis通用命令

| 命令               | 说明                                                         | 时间复杂度 |
| ------------------ | ------------------------------------------------------------ | ---------- |
| type key           | 判断key的数据类型                                            | O(1)       |
| keys [pattern]     | 获取数据库中所有符合pattern的key                             | O(n)       |
| dbsize             | 获取数据库的key的数量                                        | O(1)       |
| exists key         | 判断是否存在key;存在返回1，不存在返回0                       | O(1)       |
| del key [key...]   | 删除key;返回删除个数                                         | O(1)       |
| expire key seconds | 设置key在seconds秒后过期                                     | O(1)       |
| ttl key            | 查看key的剩余过期时间;-1 表示永远不过期,-2 表示key已经不存在了 | O(1)       |
| persist key        | 去掉key的过期时间                                            | O(1)       |

### string类型

- 结构
  - key -> value
  - age -> 20
- 注意
  - value最大大小为512MB
- API

| 命令             | 格式                     | 功能                       | 备注                                                | 复杂度 |
| ---------------- | ------------------------ | -------------------------- | --------------------------------------------------- | ------ |
| set              | set key value            | 新建/修改key-value         | 不管key是否存在都可以                               | O(1)   |
| setnx            | setnx key value          | 新建key-value              | 当key不存在才能执行                                 | O(1)   |
| set key value xx | set key value xx         | 修改key-value              | 当key存在时才能执行                                 | O(1)   |
| mset             | mset k1 v1 k2 v2 ...     | 批量设置key-value          | 如果不存在则创建；如果存在则修改                    | O(n)   |
| setrange         |                          |                            |                                                     |        |
| get              | get key                  | 获取key对应的value         | 不存在key，返回nil                                  | O(1)   |
| mget             | mget key1 key2 ...       | 批量获取key-value          | 如果key不存在，返回nil                              | O(n)   |
| getrange         | getrange key start end   | 获取字符串指定下标的所有值 |                                                     | O(1)   |
| setrange         | setrange key index value | 设置指定下标对应的值       |                                                     |        |
| getset           | getset key value         | 先获取key的value，再更新   | 如果key不存在，则返回nil，然后创建key-value         | O(1)   |
| incr             | incr key                 | 自增1                      | 如果key不存在，则创建并返回1；如果不能自增，则报错  | O(1)   |
| incrby           | incrby key n             | 自增整数n                  | 如果key不存在，则创建并返回n；如果不能自增，则报错  | O(1)   |
| incrfloat        | incrfloat key n          | 自增浮点数n                | 如果key不存在，则创建并返回n；如果不能自增，则报错  | O(1)   |
| decr             | decr key                 | 自减1                      | 如果key不存在，则创建并返回-1；如果不能自减，则报错 | O(1)   |
| decrby           | decrby key n             | 自减整数n                  | 如果key不存在，则创建并返回-n；如果不能自减，则报错 | O(1)   |
| decrfloat        | decrfloat key n          | 自减浮点数n                | 如果key不存在，则创建并返回-n；如果不能自减，则报错 | O(1)   |
| append           | append key value         | 追加value                  | 如果key不存在，则新建                               | O(1)   |
| strlen           | strlen key               | 获取key对应value长度       |                                                     | O(1)   |
| del              | del key                  | 删除key-value              |                                                     | O(1)   |

### hash类型

- 结构

  - key  -->  field | value

    ```
    user -> id   | 0
            age  | 20
            name | jack
    ```

- 特点

  - 可以看做small redis

- API

| 命令         | 格式                                                    | 功能                               | 备注                                                         | 复杂度 |
| ------------ | ------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------ | ------ |
| hset         | hset key field value                                    | 设置key对应field的vlaue            | 如果key不存在，则新建field-value                             | O(1)   |
| hsetnx       | hsetnx key field value                                  | 当不存在field时进行设置            | 如果file存在，则不执行                                       | O(1)   |
| hmset        | hmset key field1 value1 field2 value2 ... fieldN valueN | 批量设置field-value                | 果field不存在，则创建新field-value                           | O(n)   |
| hget         | hget key field                                          | 获取key对应field的value            | 如果field不存在，则返回(nil)                                 | O(1)   |
| hmget        | hmget key field1 field2 ... fieldN                      | 批量返回filed对应的value           | 如果key不存在，则返回nil                                     | O(n)   |
| hgetall      | hgetall key                                             | 返回hash key对应所有的field和value |                                                              | O(n)   |
| hexists      | hexists key field                                       | 判断是否存在field                  | 不存在，返回0；存在，返回1                                   | O(1)   |
| hdel         | hdel key field                                          | 删除key对应的field-value           | 如果key不存在，则返回0                                       | O(1)   |
| hlen         | hlen key                                                | 获取field的数量                    | 如果key不存在，则返回0                                       | O(1)   |
| hkeys        | hkeys key                                               | 获取所有的field                    |                                                              | O(n)   |
| hvals        | hvals key                                               | 获取所有的value                    |                                                              | O(n)   |
| hincr        | hincr key file                                          | 使field对应value自增1              | field不存在，创建新field-value，其value为1；如果不能自增则报错 | O(1)   |
| hdecr        | hdecr key file                                          | 使field对应value自减1              | field不存在，创建新field-value，其value为-1；如果不能自减少则报错 | O(1)   |
| hincrby      | hincrby key field num                                   | 使field对应value自增num            | field不存在，创建新field-value，其value为num；如果不能自增则报错 | O(1)   |
| hdecrby      | hincrby key field num                                   | 使field对应value自减num            | field不存在，创建新field-value，其value为-num；如果不能自减少则报错 | O(1)   |
| hincrbyfloat | hincrbyfloat key field floatnum                         | 使field对应value自增floatnum       | field不存在，则创建field-value，其value为floatnum；如果不能自增则报错 | O(1)   |



### list类型

- 结构
  - key   -->   element1- element2- ...- elementN
  - username -->  a-b-c-a-b-c
- 特点
  - 元素可以重复，有序
- API

| 命令    | 格式                                     | 功能                                            | 备注                                                     | 复杂度 |
| ------- | ---------------------------------------- | ----------------------------------------------- | -------------------------------------------------------- | ------ |
| lpush   | lpush key value1 value2 ... valueN       | 从列表左插入值(1~N个)                           | 没有则创建                                               | O(n)   |
| rpush   | rpush key value1 value2 ... valueN       | 从列表右侧插入值(1~N个)                         | 没有则创建                                               | O(n)   |
| linsert | linsert key before\|after value newValue | 从列表第一个value的前\|后插入新元素newValue     |                                                          | O(n)   |
| lpop    | lpop key                                 | 从列表左侧弹出第一个item                        |                                                          | O(n)   |
| rpop    | rpop key                                 | 从列表右侧弹出第一个item                        |                                                          | O(1)   |
| lrem    | lrem key count value                     | 根据count的值，从列表中删除count个等于value的项 | count>0，从左到右；count<0，从右到左；couont=0，删除所有 | O(n)   |
| ltrim   | ltrim key start end                      | 按照索引范围修剪列表                            | 保留[start,end]的元素                                    | O(n)   |
| lrange  | lrange key start end                     | 获取列表中指定索引范围所有的item                | 查找[start,end]的元素                                    | O(n)   |
| lindex  | lindex key index                         | 获取列表中指定索引的item                        | 支持反向索引，即负数                                     | O(n)   |
| llen    | llen key                                 | 获取列表的长度                                  |                                                          | O(1)   |
| lset    | lset key index newValue                  | 修改列表中index下标的元素为newValue             | 支持反向索引，即负数                                     | O(n)   |
| blpop   | blpop key timeout                        | 阻塞timeout秒后弹出左侧元素                     |                                                          | O(1)   |
| brpop   | brpop key timeout                        | 阻塞timeout秒后弹出右侧元素                     |                                                          | O(1)   |

### set类型

- 结构
  - key   -->  element1,elements2,...,elementN
  - subject --> chinese,music,art,...,maths 
- 特点
  - 元素不重复，无序
- API

| 命令        | 格式                         | 功能                             | 备注                                 | 复杂度 |
| ----------- | ---------------------------- | -------------------------------- | ------------------------------------ | ------ |
| sadd        | sadd key element             | 向集合中添加元素element          | 如果element已存在，则添加失败        | O(1)   |
| spop        | spop key                     | 从集合中随机弹出一个元素         |                                      | O(1)   |
| srem        | srem key element             | 将集合中key的element删除         |                                      | O(1)   |
| scard       | scard key                    | 获取集合中元素个数               |                                      | O(n)   |
| sismember   | sismember key item           | 判断item是否为集合中的元素       |                                      | O(1)   |
| srandmember | srandmember key              | 随机取出集合中的一个元素         | 是取出不是弹出，即不会破坏的原有集合 | O(1)   |
| smembers    | smembers key                 | 获取集合中所有的元素             |                                      | O(n)   |
| sdiff       | sdiff set1 set2              | 求两个集合的差集                 |                                      | O(n)   |
| sunion      | sunion set1 set2             | 求两个集合的并集                 |                                      | O(n)   |
| sinter      | sinter set1 set2             | 求两个集合的交集                 |                                      | O(n)   |
| sdiffstore  | sdiffstore newSet set1 set2  | 求两个集合的差集，结果组成新集合 |                                      | O(n)   |
| sinterstore | sinterstore newSet set1 set2 | 求两个集合的交集，结果组成新集合 |                                      | O(n)   |
| sunionstore | sunionstore newSet set1 set2 | 求两个集合的并集，结果组成新集合 |                                      | O(n)   |

### zset类型

- 结构

  - key    -->  score |  value

  ```
  ranking -->  2    | 小明
               5    | 小红
               12   | 阿强
  ```

- 特点

  - 元素不重复，有序
  - score单独可以重复，value单独可以重复
  - score-value不能重复

- API

  | 命令             | 格式                                   | 功能                                                    | 备注                                                        | 复杂度      |
  | ---------------- | -------------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------- | ----------- |
  | zadd             | zadd key score element [可以是多对]    | 添加score-element                                       |                                                             | O(logN)     |
  | zcard            | zcard key                              | 返回集合中元素的个数                                    |                                                             | O(1)        |
  | zrem             | zrem key element                       | 删除集合中指定索引的element                             |                                                             | O(1)        |
  | zincrby          | zincrby key increScore element         | 将集合中element的score增加increScore                    | 当increScore为负数时，实现减少的功能                        | O(1)        |
  | zscore           | zcore key element                      | 获取集合中element的score                                |                                                             | O(n)        |
  | zrank            | zrank key element                      | 获取集合中指定元素的排名                                |                                                             | O(1)        |
  | zrange           | zrange key start end [withscores]      | 获取集合中排行从start到end的元素                        | 以score从小到达进行排序；加上withscores会返回element的score | O(log(n)+m) |
  | zrangebyscore    | zrangebyscore key minScore maxScore    | 获取集合中score位于[minScore,maxScore]之间的元素        |                                                             | O(log(n)+m) |
  | zcount           | zcount key minScore maxScore           | 获取集合中score处于[minScore,maxScore]之间的element个数 |                                                             | O(log(n)+m) |
  | zremrangebyrank  | zremrangebyrank key start end          | 删除指定排名内的元素                                    |                                                             | O(log(n)+m) |
  | zremrangebyscore | zremrangebyscore key minScore maxScore | 删除指定分数内的元素                                    |                                                             | O(log(n)+m) |
  | zrevrank         |                                        |                                                         |                                                             |             |
  | zrevrange        |                                        |                                                         |                                                             |             |
  | zrevrangebyscore |                                        |                                                         |                                                             |             |
  | zinterstore      |                                        |                                                         |                                                             |             |
  | zunionstore      |                                        |                                                         |                                                             |             |

### 