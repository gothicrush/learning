### 架构图

![](/home/gothicrush/Desktop/learning/RabbitMQ/02. RabbitMQ 核心概念/images/2-2.png)

### 例子图

![](/home/gothicrush/Desktop/learning/RabbitMQ/02. RabbitMQ 核心概念/images/2-1.png)

### 核心概念

#### Broker

* 表示消息队列服务器实体

#### Virtual Host 

* 虚拟主机，表示一批交换器、消息队列和相关对象
* 虚拟主机是共享相同的身份认证和加密环境的独立服务器域
* 每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制

#### Message

* 消息由消息头和消息体组成
* 消息体是不透明，而消息头则由一系列的可选属性组成
* 消息头属性包括
  * routing-key：路由键
  * priority：相对于其他消息的优先级
  * delivery-mode：该消息可能需要持久性存储

#### Producer 

* 消息的生产者，也是一个向交换器发布消息的客户端应用程序。

#### Consumer

* 消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。

#### Exchange

* 交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

#### Queue 

* 消息队列

* 它是消息的容器，与交换器相连，用来保存消息直到发送给消费者
* 消息一直在队列里面，等待消费者连接到这个队列将其取走，所以也是消息的终点
* 一个消息可投入一个或多个队列

#### Binding Key

* 绑定键，用于消息队列和交换器之间的关联
* 一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表

#### Routing Key

* 路由键，用于与 Binding Key 匹配，找到对应的 Queue
* 如果消息中的 Routing Key 和 Exchange 绑定的 Binding Key 一致， 交换器就将消息发到对应的队列中

#### Connection

* 网络连接，比如一个TCP连接

#### Channel 

* 信道，多路复用连接中的一条独立的双向数据流通道
* 对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接
* 信道是建立在真实的TCP连接内地虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成

### Exchange 类型

#### direct

![](/home/gothicrush/Desktop/learning/RabbitMQ/02. RabbitMQ 核心概念/images/direct.jpg)

* 消息中的路由键如果和 Exchange 中的 binding key 一致，则交换器就将消息发到对应的队列中
* 路由键与队列名需要完全匹配，属于单播模式
* 如果一个队列绑定到交换机要求路由键为“dog”
  * 则只转发 routing key 标记为“dog”的消息
  * 不会转发“dog.puppy”，也不会转发“dog.guard”等等 

#### fanout

![](/home/gothicrush/Desktop/learning/RabbitMQ/02. RabbitMQ 核心概念/images/fanout.jpg)

* 每个发到 fanout 类型交换器的消息都会分到所有绑定到该交换器的队列上去
* fanout 交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上
* fanout 类型转发消息是最快的，因为它不处理路由键

#### topic

![](/home/gothicrush/Desktop/learning/RabbitMQ/02. RabbitMQ 核心概念/images/topic.jpeg)

* topic 交换器将路由键和 Exchange 上的 Binding Key 进行模式匹配
* 会将消息转发到所有匹配成功的队列上
* 通配符：
  * \# ：匹配0个或多个单词
  * \* ：匹配一个单词

 

 

 