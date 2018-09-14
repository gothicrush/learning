### 消息与消息中间件

* 消息是指应用之间传输的数据
* 消息中间件是一个提供高性能高可靠的消息传输交换的平台



### 消息传递的方式

* 点对点通讯 - P2P

  * 基于队列实现
  * 队列使异步通讯成为可能

  ![](/home/gothicrush/Desktop/learning/RabbitMQ/01. RabbitMQ简介/images/1-1.png)

* 发布订阅模式 - Pub/Sub

  * 有个节点，称为主题
  * 消息发布者将消息发布到这个节点上
  * 订阅者从主题中获取消息

  ![](/home/gothicrush/Desktop/learning/RabbitMQ/01. RabbitMQ简介/images/1-2.png)



### 消息中间件的作用

* 解耦
* 存储
* 削峰
* 顺序保证
* 异步通讯



### RabbitMQ下载

* http://www.rabbitmq.com/download.html
* 