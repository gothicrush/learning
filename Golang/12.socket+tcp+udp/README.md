# Part 26 - 网络编程

### 网络编程分类

* 基于 TCP/IP 的 Socket编程
* 基于 HTTP 的  HTTP 编程

### 端口

* 0是保留端口
* 1-1024是知名端口
  * 21：ftp
  * 22：ssh
  * 23：telnet
  * 24：smtp
  * 80：http
* 1025-65535是动态端口

### Socket 的使用流程

* 服务端

  * 监听端口
  * 接收客户端发送的 tcp 连接，建立与客户端的 tcp 连接
  * 创建 goroutine，处理连接请求
  * 关闭连接

* 客户端

  * 建立与服务端的连接
  * 发送请求
  * 接收服务器端返回的处理数据
  * 关闭连接

* 示意图

  ![](/home/gothicrush/Desktop/golang/my_golang笔记/4.png)

  

### Socket 实例例子

* server.go

  ```go
  package main
  
  import (
      "fmt"
      "net"
  )
  
  func process(conn net.Conn) {
      
      defer conn.Close()
      
      for {
          buf := make([]byte,1024)
          n, err := conn.Read(buf)
          if err != nil {
              fmt.Println("服务器的err=",err)
              return
          }
          fmt.Println(string(buf[:n])"客户端发送了")
      }
      
  }
  
  func main() {
      
      fmt.Println("服务器开始监听")
      // 1. tcp : 表示使用的协议是tcp
      // 2. 0.0.0.0:8888 ：表示监听的端口是8888
      listen, err := net.Listen("tcp", "0.0.0.0:8888")
      
      if err != nil {
          fmt.Println("err=", err)
          return
      }
      
      // 延时关闭资源
      defer listen.Close()
      
      for {
          // 等待客户端连接
          conn, err := listen.Accept()
          if err != nil {
              fmt.Println("err=", err)
              return
          } else {
              fmt.Printf("连接成功，客户端ip=%v",conn.RemoteAddr().String())
          }
          
          go process(conn)
      }
  }
  ```

* client.go

  ```go
  package main
  
  import (
      "fmt"
      "net"
  )
  
  func main() {
      
      conn,err := net.Dial("tcp","192.168.20.253:8888")
      
      if err != nil {
          fmt.Println("err=", err)
      }
      
      defer conn.Close()
      
      reader := bufio.NewReader(os.Stdin)
      line, err := reader.ReadString("\n")
      
      if err != nil {
          fmt.Println("err=", err)
      }
      
      n, err := conn.Write([]byte(line))
      
      if err != nil {
          fmt.Println("err=", err)
      }
      
      fmt.Printf("客户端发送了 %d 个字节，并退出", n)
  }
  ```
