# goroutine(协程)

### Go的进程和Go的协程

* Go进程：
  * 一个Go进程上，可以启动多个协程
* Go协程：
  * 有独立栈空间
  * 共享堆空间
  * 调度由用户定义
  * 协程是轻量级线程
* Go协程对比其他语言并发实现的优势
  * Go协程是进程开启的，是轻量级的线程，对资源耗费小
  * 其他语言的并发一般是基于线程的
  * Go协程可以轻松开上万个，而其他语言开上万个并发较为吃力

### Go协程的使用

```go
// 编写一个程序，满足以下功能：
// 1. 在进程中，开启一个 goroutine，该协程每隔1秒输出 "hello world"
// 2. 在进程中，也每隔1秒输出 "hello golang"，输出10次后，进程结束
// 3. 要求进程和 goroutine 同时执行

package main

import (
    "fmt"
    "time"
    "strconv"
)

func test() {
    for i := 0; i < 10; i++ {
        fmt.Println("[test] hello world" + strconv.Itoa(i))
        time.Sleep(time.Second)
    }
}

func main() {
    
    go test() // 开启一个协程，执行这个 test
    
    for i := 0; i < 10; i++ {
        fmt.Println("[main] hello golang" + strconv.Itoa(i))
        time.Sleep(time.Second)
    }
}
```

### goroutine协程的调度模型

* MPG模型
* M：操作系统上的进程(主线程)
* P：协程需要的上下文
* G：协程

### 查看和设置Go运行的cpu数目

* 概述
  * go1.8以后，默认让程序运行多个核，可以不设置
  * go1.8之前，最好进行设置，从而提高性能

* 查看系统的逻辑cpu数目

  ```go
  import "runtime"
  
  fmt.Println(runtime.NumCPU())
  ```

* 设置执行Go程序的cpu个数

  ```go
  import "runtime"
  
  runtime.GOMAXPROCS(num) // 本函数在编译优化时会被去掉
  ```

#### goroutine中使用 recover

* goroutine中使用 recover，可以解决某个协程中出现 panic，导致整个程序崩溃的问题

  ```go
  defer func() {
      if err := recover(); err != nil {
          fmt.Println("发生错误", err)
      }
  }
  ```

  

# channel

### goroutine资源竞争的问题

```go
// 下面程序会发成 资源竞争问题(concurrent map writes)
// 查看是否发生资源竞争问题可以用命令：go build -race test.go
// 现采用goroutine计算1-200各个数的阶乘，并把每个数的阶乘放进map中，最后显示出来

package main

import (
    "fmt"
    "time"
)

var (
    myMap = make(map[int]int, 10)
)

func calculate(n int) {
    
    res := 1
    for i := 1; i <= n; i++ {
        res *= i
    }
    
    myMap[n] = res
}

func main() {
    for i := 1; i <= 200; i++ {
        go test(i)
    }
    
    time.Sleep(20 * time.Second) // 避免goroutine还没执行，主线程就结束了
    
    for k,v := range myMap {
        fmt.Printf("map[%d]%d\n", k, v)
    }
}
```

### 解决资源竞争问题 - 通过全局变量加锁

```go
package main

import (
    "fmt"
    "time"
    "sync"
)

var (
    myMap = make(map[int]int, 10)
    lock sync.Mutex // 全局的互斥锁
)

func calculate(n int) {
    
    res := 1
    for i := 1; i <= n; i++ {
        res *= i
    }
    
    lock.Lock()    // 加锁
    myMap[n] = res
    lock.Unlock()  // 解锁
}

func main() {
    for i := 1; i <= 200; i++ {
        go calculate(i)
    }
    
    time.Sleep(20 * time.Second) // 避免goroutine还没执行，主线程就结束了
    
    lock.Lock()    // 加锁
    for k,v := range myMap {
        fmt.Printf("map[%d]%d\n", k, v)
    }
    lock.Unlock()  // 解锁
}
```



### 解决资源竞争问题 - 通过channel

#### 全局变量锁的缺点

* 主线程等待所有 goroutine 全部完成的时间很难确定
* 通过全局变量锁实现的同步，不利于多个协程对全局变量的读写操作

####  channel 介绍

* channel 是一个引用类型
* channel的本质是一个队列
* 数据是先入先出的，即多个 goroutine 同时访问时，也不需要加锁
* channel是有类型的，一个string类型的channel只能存放string

#### channel 声明

```go
var 变量名 chan 数据类型

var intChan chan int
var mapChan chan map[int]string
var perChan chan Person
```

#### channel 初始化

* channel 必须初始化后才能使用，即 make 后

```go
var intChan chan int = make(chan int, 3)
```

#### channel 写入数据

```go
intChan <- 10

// channel数据放满后就不能再放了，容量不是动态增长的
```

#### channel读取数据

```go
var num int
num = <- intChan

// 在没有使用协程的情况下，当channel已经为空，但是依然继续取时，会报 deadlock
```

#### channel长度和容量

```go
len(intChan)
cap(intChan)
```

#### channel关闭

```go
// 关闭 channel 后，就不能再往里面写数据了，但如果 channel 里面还有数据，则可以继续读取
close(intChan)
```

#### channel遍历

```go
// 在遍历时，如果 channel 还没关闭，则会报 deadlock
// 在遍历时，如果 channel 已经关闭，则遍历正常执行

for v := range intChan {
    fmt.Println("v=", v)
}
```

#### channel阻塞机制

```go
如果只向 channel 中写入数据而不读取，导致 channel 满了，还继续写，就会出现阻塞而 deadlock

如果读的频率远低于写的频率也没有问题，也不会发生 deadlock，关键是要去读
```

#### channel 只读与只写

```go
var chan1 chan int // 可读可写
var chan2 chan <- int // 只写
var chan3 <- chan int // 只读
```

#### channel与select

```go
// 使用 select 可以解决从管道取数据阻塞的问题

// 1. 定义一个管道 10个数据int
intChan := make(chan int, 10)
for i := 0; i < 10; i++ {
    intChan <- i
}
// 2. 定义一个管道  5个数据string
strChan := make(chan string, 5)
for i := 0; i < 10; i++ {
    intChan <- "hello" + fmt.Sprintf("%d", i)
}

// 在 for-range 遍历管道时，如果不关闭管道，会发生 deadlock 现象
// 但是我们可能不好确定什么时候该关闭管道
// 可以使用 select 方法解决
for {
    select {
        // 这里，如果管道没有关闭，也不会因为一直阻塞而 deadlock
        // 会自动到下一个 case 匹配
        case v := <- intChan:
            fmt.Println("从 intChan 读取数据%v\n", v)
        case v :=  <- strChan:
            fmt.Println("从 strChan 读取数据%v\n", v)
    	default:
        	fmt.Println("都没取到")
    }
}
```

### 通过 channel 解决素数问题

#### 思路图示

![](/home/gothicrush/Desktop/golang/my_golang笔记/5.png)

#### 代码实现

```go
package main
import (
    "fmt"
)

func putNum(intChan chan int) {
    
    for i := 0; i < 1000; i++ {
        intChan <- i
    }
    
    close(intChan)
}

func primeNum(intChan chan int, primeChan chan int, exitChan chan bool) {
    var flag bool
    for {
        num, ok := <- intChan
        
        if !ok {
            break
        }
        
        flag = true
        for i := 2; i < num; i++ {
            if num % i == 0 {
                flag = false
                break
            }
        }
        
        if flag {
            primeChan <- num
        }
    }
    
    fmt.Println("有一个primeChan因为取不到数了而退出")
    exitChan <- true
}

func main() {
    
    intChan := make(chan int, 1000)
    primeChan := make(chan int, 1000)
    exitChan := make(chan bool, 4)
    
    go putNum(intChan)
    
    for i := 0; i < 4; i++ {
        go primeNum(intChan, primeChan, exitChan)
    }
    
    go func() {
        for i := 0; i < 4; i++ {
            <- exitChan
        }

        close(primeNum)
    }()
    
    for {
        res, ok := <- primeNum
        if !ok {
            break
        }
        fmt.Printf("素数：%d\n", res)
    }
    
    fmt.Println("main线程退出")
}
```
