## TCP协议

### TCP协议特点

* 面向连接
  * 应用程序在使用TCP协议之前，必须先建立TCP连接
  * 在数据传输完毕后，必须释放已经建立的TCP连接
  * 类似于打电话
* 点对点通信
  * TCP协议就像打电话，只能一对一，不能一对多或多对多
* 可靠传输
  * TCP协议能够保证传输的数据无差错，不丢失，不重复，不乱序
* 全双工通信
  * TCP协议允许双方同时接收和发送数据
* 面向字节流
  * TCP协议传输的数据以字节为单位，以流的形式传输

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE1.PNG)



### TCP报文首部格式

* 结构图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE2.PNG)

* 序号示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE3.PNG)

| 组成     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 序号     | 每个数据包的第一个字节位置在全数据中的位置                   |
| 源端口   | 发送方端口                                                   |
| 目的端口 | 接收方端口                                                   |
| 确认号   | 期待接收的下一个数据包的序号                                 |
| 数据偏移 | 指TCP报文数据部分开始位置，即TCP报文首部占用空间大小<br />数据偏移占4位，单位为"4字节"，即TCP报文首部最大为15*4=60字节<br />而TCP报文首部固定部分大小为20字节，即TCP首部可选部分最大为40字节 |
| 保留     | 保留，暂时没用                                               |
| 紧急URG  | 告诉发送发应优先发送                                         |
| 确认ACK  | ACK=1时有效，用于建立TCP连接                                 |
| 推送PUSH | 告诉接收方应优先接收                                         |
| 窗口     | 占2字节，范围[0，2^16-1]，用于TCP流量控制和拥塞控制          |
| 检验和   | 占2字节，计算检验和时，需要在TCP报文段前加上12字节的伪首部   |
| 紧急指针 | 仅在URG=1时有效，指明紧急数据的位置                          |
| 选项     | 长度可变，只有一种选项：**MSS**（Maximum Segment Size），最大报文段长度 |





### TCP可靠传输

#### 停止等待协议

* 概念

  * 发送方发送一个数据包，接收方接收后，返回一个确认包
  * 发送方只有当收到确认包后才发送下一个数据包
  * 现在已经不用这种技术，因为效率太低

* 丢包情况

  * 接收方没有受到数据包
    * 发送方超时重传
  * 接收方收到数据包，但发送方没有收到确认包
    * 发送方超时重传，接收方收到重复包后丢弃，返回确认包
  * 接收方收到数据包，但发送方超时收到确认包
    * 发送方超时重传，接收方收到重复包后丢弃，返回确认包

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE4.PNG)

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE5.PNG)



#### 连续ARQ协议和滑动窗口协议

* 概念

  * 停止等待协议的改进版：一包一确认效率太低
  * 核心思想：多个包分为一组，收到一个组才进行一次确认
  * 一组的大小就是TCP窗口的大小
  * 现在主要使用这种技术

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE6.PNG)

* 流程图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE7.PNG)

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE8.PNG)

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE9.PNG)

* 选择性确认

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE10.PNG)

* 超时重传时间计算

  * 超时重传时间取决于TCP往返传输时间（**RTT**）
  * RTT计算方法1：TCP Timestamp
    * 在发送时放入发送时间点的时间戳，接收方收到时，用接收时间点时间戳减去发送时间点时间戳
  * RTT计算方法2：重传队列中数据包的TCP控制块
    * **不懂不懂**



### TCP流量控制

* 概念：流量控制是指TCP连接的两端（发送方，接收方）在进行数据传输时，控制其发送和接收数据包的个数

* 核心思想

  * TCP协议报文有窗口，发送方叫发送窗口，接收方叫接收窗口
  * TCP传输过程中，发送窗口和接收窗口都会动态改变其大小，从而改变一次发送数据包的个数和一次接收数据包的个数，最终实现流量控制

* 流程图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE11.PNG)

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE12.PNG)

### TCP拥塞控制

* 概念：TCP拥塞控制的参与者是整个网络所有节点，所有节点共同采取措施避免网络的拥塞

#### 慢开始算法

* 概念：发送方在发送数据包时，一次发送的数量(**MSS**)随发送次数逐渐增多（第n次发送2^n个）

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE13.PNG)



#### 拥塞控制方案一

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE14.PNG)



#### 快重传算法

* 概念：

  * 接收方每收到一个失序的分组后立即发送重复确认，这是为了使发送方能够及早知道分组没有及时到达而不要等到自己发送数据时才进行确认
  * 快重传规定：发送方只要一连收到三个重复确认就要立即重传没有准确送达的报文段，而不要等待超时重传时间

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE15.PNG)

#### 拥塞控制方案二

* 快重传配合快恢复算法

![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE16.PNG)

* 发送窗口 = min[接收窗口，拥塞窗口] 



### TCP连接管理

#### 连接建立 - 握手

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE17.PNG)

* 为什么需要3次握手

  * 背景：假设是2次握手
  * A申请与B握手，A --> B
  * 第一次：A发出建立连接请求r1，但是路由的路线很慢
  * 第二次：因为请求r1走的很慢，超时都不到达，所以A放弃第一次连接请求，发出第二次请求r2
  * B收到r2请求，并发出响应包q2，A收到响应包q2，至此，两次握手成功，连接建立
  * 过了一会，B收到r1，并发出响应包q1，A收到响应包q1，但是A之前已经放弃了第一次请求
  * 由于两次握手的原因，B认为已经建立了连接，而A不认为是建立了连接，所以B就一直维持连接状态

#### 连接释放 - 挥手

* 示意图

  ![](https://github.com/gothicrush/learning/blob/master/Network/05.%E4%BC%A0%E8%BE%93%E5%B1%82/images/%E5%9B%BE18.PNG)

* 为什么要等待2**MSL**（Maximum Segment Lifetime：最大报文生存时间）

  * 场景

    ```bash
    A主动向B请求断开
    
    1: A --> FIN --> B
    2: A <-- ACK <-- B
    # 此时A --> B连接已关闭
    3: A <-- FIN <-- B
    4: A --> ACK --> B
    ```

  * 等待2MSL的原因是为了确保 `A->ACK->B` 能够送达

  * 如果`A->ACK->B`丢失 -->  B认为`A<-FIN<-B`丢失，进行重传

  * A接收到`A<-FIN<-B`重传所需时间：`A<-FIN<-B`超时时间 + `A<-FIN<-B`传输时间

  * 为了保守与容易计算，保留两个**MSL**来确保A能接收到`A<-FIN<-B`重传包

  * 一旦A接收到`A<-FIN<-B`，就重新等待2MSL

  * 如果A在2MSL都没有收到`A<-FIN<-B`，则有两种情况

    * B已经收到了A的包：可以关闭
    * B端网络断开了：可以关闭



### SYN 攻击

* 攻击者频繁地与服务器建立连接，造成服务器带宽资源与计算机资源被耗尽



## UDP协议

### UDP协议特点

* 面向无连接：发送数据前不需要建立连接
* 不保证可靠传输：因为不建立连接
* 面向报文传输
* 支持一对一，一对多，多对多通信
* 首部开销小，只占8个字节，伪首部占12字节
* 不具有拥塞控制功能



### UDP报文结构

* 图中**16位UDP长度**和**UDP长度**相同

![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\udp.jpg)

### 

### UDP校验和计算方法

* UDP包的伪首部和首部和数据报文全部按16位进行划分
* 然后将它们进行求和【此时参与计算的检验和为0】
* 然后结果取反码，得到真正的检验和
* 最后将其放入**检验和**中



### UDP检验和使用

* UDP包的伪首部和首部和数据报文全部按16位进行划分
* 然后将它们进行求和【此时参与计算的检验和为0】
* 如果结果为全1，则数据传输没错，否则数据传输出错



### DDos攻击

* 攻击者同时发送大量的UDP包给服务器