## IP协议

### IP协议全称

* Internet Protocol
* 互联网互连协议

### IP协议作用

* 实现数据在网络节点上互相传输

### IP协议特点

* 不面向连接
* 不保证可靠

### IP协议数据报结构

![](https://github.com/gothicrush/learning/blob/master/Network/04.%20%E7%BD%91%E7%BB%9C%E5%B1%82/03.%20IP%E5%8D%8F%E8%AE%AE%E7%B0%87/images/IP%E5%8D%8F%E8%AE%AE%E6%95%B0%E6%8D%AE%E6%8A%A5.PNG)

| 组成     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 版本     | 目前有IPv4和IPv6两种版本                                     |
| 首部长度 | 单位4字节，所以首部长度最大为 15 * 4 = 60字节                |
| 区分服务 | 不同服务的优先级不同，从而保证服务质量保证(QoS)              |
| 总长度   | IP首部与数据部分字节之和，最大为 2^16-1个字节                |
| 片偏移   | 占13位，本分片再原始数据报文中相对首位的偏移位               |
| 生存时间 | TTL，指数据包跳转次数剩余，最多255跳                         |
| 协议     | 指明上层使用的协议，从而确定要给哪个进程                     |
| 标识     | IP协议软件中的一个数，用于唯一标识主机发送的数据报<br />在发出IP报文时，每个分片会被分配一个数<br />每分配一个报文，这个数加1，溢出则从0开始算<br />标识用于分片达到目的主机后进行重组 |
| 标志     | 最低位(MF)：MF=1表示后面还有分片，MF=0表示已经是最后一个分片<br />中间位(DF)：DF=1表示不允许分片，DF=0表示允许分片 |

* 片偏移计算

  ![](https://github.com/gothicrush/learning/blob/master/Network/04.%20%E7%BD%91%E7%BB%9C%E5%B1%82/03.%20IP%E5%8D%8F%E8%AE%AE%E7%B0%87/images/%E7%89%87%E5%81%8F%E7%A7%BB%E8%AE%A1%E7%AE%97.PNG)

* 片偏移示例

  ![](https://github.com/gothicrush/learning/blob/master/Network/04.%20%E7%BD%91%E7%BB%9C%E5%B1%82/03.%20IP%E5%8D%8F%E8%AE%AE%E7%B0%87/images/%E7%89%87%E5%81%8F%E7%A7%BB%E7%A4%BA%E4%BE%8B.PNG)



## ICMP协议

### ICMP协议全称

* Internet Control Message Protocol - 互联网控制报文协议

### ICMP协议作用

* 用于主机、路由器之间传递控制信息
* 控制信息指：网络是否流畅，路由器是否可用，主机是否可达

### ICMP协议使用

* ICMP数据报一般被TCP/UDP协议使用，也可以被用户进程使用
* 命令行使用：`trace IP地址`，即可看到IP数据包沿途所经过的路由器的信息

### ICMP协议数据报传输

* ICMP报文被封装再IP数据报内，随IP数据报进行传输

![](https://github.com/gothicrush/learning/blob/master/Network/04.%20%E7%BD%91%E7%BB%9C%E5%B1%82/03.%20IP%E5%8D%8F%E8%AE%AE%E7%B0%87/images/ICMP%E6%95%B0%E6%8D%AE%E6%8A%A5.PNG)

### ICMP报文类型和代码

![](https://github.com/gothicrush/learning/blob/master/Network/04.%20%E7%BD%91%E7%BB%9C%E5%B1%82/03.%20IP%E5%8D%8F%E8%AE%AE%E7%B0%87/images/ICMP%E6%8A%A5%E6%96%87%E7%B1%BB%E5%9E%8B.PNG)



## ARP协议

### ARP协议全称

* Address Resolution Protocol
* 地址解析协议

### ARP协议作用

* 用于将以太网（局域网）中的IP地址解析为MAC地址
* 因为当一个IP数据报来到一个局域网时，它不知道应该去哪个计算机，于是使用ARP协议发出一个广播，内容是`你们谁的IP地址是xxx.xxx.xxx.xxx的，告诉我你的MAC地址，我发数据给你`，目标计算机收到广播后，用ARP协议广播`我是啊，我的MAC地址是yyyy`
* 点到点链路使用PPP协议，不需要ARP协议

### ARP协议使用

* 命令行使用：`arp -a`，看到本机存储的`IP地址 <--> MAC地址`的映射表



## IGMP协议

### IGMP协议全称

* Internet Group Manage Protocol
* 互联网组管理协议

### IGMP协议作用

* 用于组播功能的实现
* 主机通过IGMP协议通知路由器希望接收或离开某个特定的组播组
* 路由器通过IGMP协议周期性查询组播组中成员状态，维护组播组中成员的关系

### 组播是什么

* 在发送者与每一个接收之间实现一对多的网络连接
* 如果发送者需要给多个接收者发送相同的数据，只需要发送一份数据就行
* 不用针对每个接收方都发送一份，提高了通信效率，减少资源使用