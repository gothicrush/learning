## 慢查询

### 慢查询是什么

* 慢查询本质是慢查询日志，它记录了一些执行速度很慢的命令

### 慢查询与生命周期

* 生命周期

  ```
   -------                   ------------------------------------------    
  |       |   1.发送请求     | redis服务端                              | 
  |   客  |  -------------->|              2.排队                      | 
  |   户  |                 |   cmd5->cmd4->cmd3->cmd2->cmd1           |
  |   端  |    4.返回结果    |                            |             |  
  |       | <---------------|                        3.执行命令         |
   -------                   ------------------------------------------
  ```

  

* 慢查询发生在第3阶段
* 客户端超时不一定是慢查询导致，慢查询是客户端超时的一个可能性

### 慢查询日志保存形式

* 以队列方式存储，队列长度固定，一旦新的慢查询命令过多，最初的慢查询命令会从日志中删除
* 慢查询日志保存在内存中，如果有必要，需要自己进行持久化

### 慢查询两大参数

* slowlog-log-slower-than
  * 慢查询阈值，单位微秒，大于这个阈值的命令，被判定为慢查询，会加入慢查询日志中
  * 这个值=0，记录所有命令
  * 这个值<0，不记录任何命令
  * 默认值：10000
* slowlog-max-len
  * 慢查询日志队列长度
  * 默认值：128

### 设置慢查询参数

* 通过配置文件
* 动态配置
  * config set slowlog-max-len 1000
  * config set slowlog-log-slower-than 1000

### 慢查询命令

* slowlog get[n]
  * 获取慢查询队列中某个值
* slowlog len
  * 获取慢查询队列的长度
* slowlog reset
  * 清空慢查询队列
* 可以配合上述命令对慢查询日志进行持久化



## 发布订阅

### 角色

* 发布者 publisher
* 订阅者 subscriber
* 频道 channel

### 模型

![](C:\Users\narli\Desktop\learning\Redis\04.Redis慢查询与发布订阅与流水线\images\pubsub.PNG)

### 命令

* publish channel message

  ```bash
  publish sohu:tv "hello world"
  ```

* subscribe [channel]...

  ```bash
  subscribe sohu:tv
  ```

* unsubscribe [channel]...

  ```bash
  unsubscribe sohu:tv
  ```

* psubscribe [pattern]

  ```bash
  通过模式去订阅
  ```

* punsubscribe [pattern]

  ```bash
  取消匹配模式的订阅
  ```

* pubsub channels

  ```bash
  列出至少有一个订阅者的频道
  ```

* pubsub numsub [channel]...

  ```bash
  列出指定频道订阅者的数量
  ```

  

## 流水线 pipeline

### 网络传输时间与命令时间

* 请求处理时间 = 网络传输时间 + 命令时间
* 网络传输时间：客户端与服务端之间数据在网络传输过程中所消耗的时间
* 命令时间：服务端处理命令所耗费的时间
* 命令时间是很快的，所以请求处理时间主要取决于网络传输时间

### pipeline是什么

* pipeline可以将客户端多个命令进行打包，然后一次性通过网络传输到redis服务端，redis处理后，再一次性通过网络返回处理结果
* 即 1次请求处理时间 = 1次网络请求时间 + n次命令时间

### pipeline作用

* 减少网络请求时间

### pipeline注意事项

* redis中M操作(mset,mget等等)是原子性的

* pipeline不是原子性的

  ```go
  其他命令->其他命令->pipeline子命令->其他命令->pipeline子命令->其他命令 ===>>> Server
  ```

* pipeline每次只能作用在一个redis节点上



## 事务

### redis事务

* redis中单个命令是原子性的，但事务的执行并不是原子性的
  * 如果事务中间某条命令失败，也不会回滚，依旧执行下一条命令
* redis开始事务后，命令不会立即执行，而是进入一个队列中，当提交事务后才会执行
  * 事务可以看作是一个批量执行脚本

### 事务的使用

* 开始事务：MULTI
* 命令入队
* 执行事务：EXEC