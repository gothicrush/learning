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



### RabbitMQ下载与安装

* 因为 RabbitMQ 是用 erlang开发的，所以必须先准备好 erlang环境

#### erlang下载

* 下载地址：http://www.erlang.org/downloads

* 选择合适的版本进行下载

  ![](/home/gr/learning/RabbitMQ/01. 消息中间件简介 + RabbitMQ下载安装/images/erlang安装.png)

#### erlang安装

* 安装依赖

  ```bash
  yum install gcc glibc-devel make ncurses-devel openssl-devel xmlto unixODBC
  ```

* 解压

  ```bash
  tar -zxvf otp_src_20.2.tar.gz
  ```

* 环境检查

  ```bash
  cd otp_src_20.2
  ./configure --prefix=安装路径 --without-javac
  ```

* 编译安装

  ```bash
  make && make install
  ```

* 环境变量设置

  ```bash
  PATH="$PATH:/erlang安装路径/bin"
  ```

* 测试

  ```bash
  erl
  ```

#### RabbitMQ下载

* 下载地址：http://www.rabbitmq.com/
* 选择合适版本，注意要符合erlang版本

![](/home/gr/learning/RabbitMQ/01. 消息中间件简介 + RabbitMQ下载安装/images/rabbitmq下载.jpg)

#### RabbitMQ安装

* 解压

  ```bash
  xz -d rabbitmq.tar.xz
  tar -xvf rabbitmq.tar
  ```

* 设置环境变量

  ```bash
  PATH="$PATH:/解压目录/sbin"
  ```

* 测试

  ```bash
  rabbitmqctl status
  ```
