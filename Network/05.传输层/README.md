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

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图1.PNG)



### TCP报文首部格式

* 结构图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图2.PNG)

* 序号的含义每个数据包的第一个字节位置在全数据中的位置

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图3.PNG)

* 源地址：发送方IP地址

* 目的地址：接收方IP地址

* 确认号：期待接收的下一个数据包的序号

* 数据偏移：

  * 指TCP报文数据部分开始位置，即TCP报文首部占用空间大小
  * 占4位，单位为"4字节"，即TCP报文首部最大为15*4=60字节
  * 而TCP报文首部固定部分大小为20字节，即TCP首部可选部分最大为40字节

* 保留：保留，暂时没用

* 紧急URG：告诉发送发应优先发送

* 确认ACK：ACK=1时有效，用于建立TCP连接

* 推送PUSH：告诉接收方应优先接收

* 窗口：占2字节，范围[0，2^16-1]，用于TCP流量控制和拥塞控制

* 检验和：占2字节，计算检验和时，需要在TCP报文段前加上12字节的伪首部

* 紧急指针：仅在URG=1时有效，指明紧急数据的位置

* 选项：长度可变，只有一种选项：**MSS**（Maximum Segment Size），最大报文段长度



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

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图4.PNG)

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图5.PNG)



#### 连续ARQ协议和滑动窗口协议

* 概念

  * 停止等待协议的改进版：一包一确认效率太低
  * 核心思想：多个包分为一组，收到一个组才进行一次确认
  * 一组的大小就是TCP窗口的大小
  * 现在主要使用这种技术

* 丢包情况

* 示意图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图6.PNG)

* 流程图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图7.PNG)

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图8.PNG)

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图9.PNG)

* 选择性确认

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图10.PNG)

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

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图11.PNG)

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图12.PNG)

### TCP拥塞控制

* 概念：TCP拥塞控制的参与者是整个网络所有节点，所有节点共同采取措施避免网络的拥塞

#### 慢开始算法

* 概念：发送方在发送数据包时，一次发送的数量(**MSS**)随发送次数逐渐增多（第n次发送2^n个）

* 示意图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图13.PNG)



#### 拥塞控制方案一

* ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图14.PNG)



#### 快重传算法

* 概念：

  * 接收方每收到一个失序的分组后立即发送重复确认，这是为了使发送方能够及早知道分组没有及时到达
  * 而不要等到自己发送数据时才进行确认
  * 快重传规定：发送方只要一连收到三个重复确认就要立即重传没有准确送达的报文段，而不要等待超时重传时间

* 示意图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图15.PNG)

#### 拥塞控制方案二快恢复算法

![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图16.PNG)





#### 拥塞避免方法



发送窗口 = min[接收窗口，拥塞窗口] 



### TCP连接管理

#### 连接建立 - 握手

* 示意图

  图图16-1

* 为什么需要3次握手

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图17.PNG)

* 总结

  第一次：SYN=1,ACK=0

  第二次：SYN=1,ACK=0

  第三次：SYN=0,ACK=1

#### 连接释放 - 挥手

* 示意图

  ![](C:\Users\narli\Desktop\learning\Network\05.传输层\images\图18.PNG)

* 等待2MSL的原因



### SYN 攻击



## UDP协议