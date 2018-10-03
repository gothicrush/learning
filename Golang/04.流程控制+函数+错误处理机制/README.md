# 流程控制

### 分支流程控制

* 单分支

  ```go
  if 条件表达式 { // { } 必须要有，且左大括号不能换行
      //
  }
  ```

* 双分支

  ```go
  if 条件表达式 { // { } 必须要有，且左大括号不能换行
      
  } else {      // { } 必须要有，且左大括号不能换行，else必须和右大括号同行
       
  }
  ```

* 多分支

  ```go
  if 条件1 {          // { } 必须要有，且左大括号不能换行
      
  } else if 条件2 {   // { } 必须要有，且左大括号不能换行，else必须和右大括号同行
      
  } else if 条件n {   // { } 必须要有，且左大括号不能换行，else必须和右大括号同行
      
  } else {           // { } 必须要有，且左大括号不能换行，else必须和右大括号同行
                           // 多分支中 else 不是必须的
  }
  ```

* if 语句可以初始化变量

  ```go
  if ok := function(); ok {
      
  }
  ```

* switch语句

  ```go
  switch 表达式 {
      case 表达式1,表达式2,...:
        语句1
      case 表达式3:
      	语句2
      default:
      	语句3
  }
  
  
  // case的表达式要求不能重复，case表达式值与switch表示值需要是同一类型
  // case后面可以跟多个表达式，用逗号分隔
  // 一旦匹配，就不再进行下一步匹配
  // default不是必须的，可有可无
  // 如果匹配不到，则执行default块(如果有)
  // 匹配选项后面不用加 break，默认不贯穿
  // 如果想要穿透，则用 fallthrough 声明
  // switch后也可以不跟表达式，效果等同 switch True {}
  // switch后可以先声明一个变量再使用 switch i:=100 {}
  ```

* if-else if-else 与 switch 使用场景对比

  ```go
  // 对于匹配区间
  推荐用 if-else if-else
  // 对于匹配具体值
  推荐用 switch
  ```

### 循环分支流程控制

* Go中只有 for 没有 while 也没有 do-while

* for 使用方法-1 普通 for 循环

  ```go
  for 循环变量初始化; 循环条件; 循环条件迭代 {
      // 循环操作语句
  }
  ```

* for 使用方法-2 模拟 while 循环

  ```go
  for 循环条件 {
      // 循环操作语句
  }
  ```

* for 使用方法-3 死循环

  ```go
  for {
      // 死循环
  }
  
  // 等价于
  for ;; {
      // 死循环
  }
  ```

* for 使用方法-4 for-range

  ```go
  for index,val := range str {
      fmt.Println(index, val)
  }
  ```

* for使用方法-5 新形式

  ```go
  for 初始化; 循环条件 {
      // 循环操作语句
  }
  ```

  

* for 注意事项

  * 循环条件返回的 bool 的表达式

  * break 可以根据标签跳出

    ```go
    break         //跳出最近的一层
    break label1  //跳到label1指定位置
    break label2  //跳到label2指定位置
    
    for i := 0; i < 4; i++ {
        label1:
        for j :=0; j < 10; j++ {
            if j == 2 {
                break label1
            }
            fmt.Println("j =", )
        } 
    }
    ```

  * continue 可以根据标签跳出

    ```go
    continue         //跳出最近的一层
    continuecontinue label1  //跳到label1指定位置
    break label2  //跳到label2指定位置
    
    for i := 0; i < 4; i++ {
        label1:
        for j :=0; j < 10; j++ {
            if j == 2 {
                continue label1
            }
            fmt.Println("j =", )
        } 
    }
    ```


### goto语句

* 不推荐使用 goto 语句

* 语法

  ```go
  label: 语句
  
  goto label
  ```


# 函数

### 定义函数

```go
func 函数名 (形参列表) (返回值列表) { // 左大括号不能换行
    //执行语句
    return xxx
}
```

### 函数参数简化

```go
func test(a int, b int, c int) {}
等价于
func test(a,b,c int) {}
```

### 函数返回值

* Go语言可以返回多个值，如果返回不想接收，可以用 _ 来省略

* 如果只返回一个值，则返回值列表可以不加()

* 不指定返回值名称

  ```go
  func add(a int, b int) int {
      result = a + b
      return result
  }  
  ```

* 指定返回值名称

  ```go
  func add(a int, b int) (result int) {
      result = a + b
      return
  }
  ```

### 可变参数

* 会把不定参数封装为一个slice
* 如果一个函数的形参列表中有可变参数，则可变参数需要放在形参列表最后

```go
func sum (args... int) total int {
    for _, val := range args {
        total += val
    }
    return
}

sum(1,2,3,4,5,6,7)
```



### 函数注意事项

* 基本数据类型和数组默认是值传递的，即进行值拷贝，在函数内部修改，不会影响到原来的值

* 如果希望函数内部修改可以影响到外面，则需要使用指针

* Go函数不支持函数重载，不支持嵌套，不支持默认参数

* Go函数本身也是一种数据类型，可以将函数作为参数传递

  ```go
  func getSum(n1 int,n2 int) int {
      return n1 + n2
  }
  
  func myFunc(funvar func(int,int) int, num int, num2 int) int {
      return funvar(num1, num2)
  }
  ```

* 为函数类型起别名

  ```go
  type myf func(int,int) int
  ```

### init函数

* 名字固定，每一个Go源文件中都可以包含这样的一个函数，该函数在main函数执行前，会先被调用

* 该函数可以用作初始化操作

  ```go
  func init() { // 无形参，无返回值
      
  }
  ```

* 如果有全局变量定义，则执行顺序为

  ```go
  全局变量定义 -> init函数 -> main函数
  ```

### 匿名函数

* 只调用一次的匿名函数

  ```go
  res := func (n1 int, n2 int) int {
      return n1 + n2
  }(1,2)
  ```

* 可调用多次的匿名函数

  ```go
  a := func (n1 int, n2 int) int {
      return n1 + n2
  }
  
  a(1,2)
  a(3,4)
  a(5,6)
  ```

* 全局匿名函数

  ```go
  var (
      gFun = func(n1 int, n2 int) int {
          return n1 + n2
      }
  )
  ```

### 闭包

```概念
闭包就是 一个函数A中定义另外一个函数B
且函数B中有使用到函数A中的局部变量，且函数A将函数B返回
```

### defer

* 作用

  ```go
  defer是函数的延时执行机制
  defer语句当函数结束时才调用
  defer语句一般用于释放资源
  ```

* 注意

  ```go
  多个defer语句会放到一个栈中
  即最先声明的defer语句会在最后执行
  当语句入栈时，也会将相关的值进行拷贝
  ```

### 函数参数传递方式

* Go语言一种有两大类数据类型：值类型 ；引用类型
* Go中值类型包括：int系列，float系列，complex系列，bool，string，数组和结构体
* Go中引用类型包括：slice，map，chan，interface，指针
* 值类型的默认传递方式是值传递，引用类型的默认传递方式是引用传递

### 内置函数

* len：用来求长度，如string，array，slice，map，channel

* new：用来分配内存，主要用于分配值类型

  ```go
  numPtr := new(int) 这个numPtr是 *int 类型
  ```

* make：用来分配内存，主要用于分配引用类型

* cap：求切片的容量


# 错误处理机制

### panic-defer-recover机制

* 默认情况下，当发生错误(panic)后，程序就会崩溃
* 如果希望当发生 panic 后，可以捕捉到 panic，并进行处理，使程序不崩溃，则需要进行错误处理
* Go语言不支持传统的 try...catch....finally，而是使用 defer, panic, recover机制
* 流程：抛出一个panic，然后在 defer 语句中通过 recover 函数捕获这个 panic，并处理 

 ```go
func main() {
    
    test()
    fmt.Println("尽管发生了 panic，程序还是继续执行了")
    
}

func test() {
    defer func() {
        err := recover() // recover内置函数，可以捕获异常
        if err != nil {
            fmt.Println("err=",err)
        }
    }()
    
    num1 := 10
    num2 := 0
    res := num1 / num2
    fmt.Println(res)
}
 ```

### 自定义异常

* 使用errors.New和panic内置函数

  * error.New("错误说明")，返回一个 error 类型的值，表示一个错误
  * panic()，内置函数，接收任意值作为参数，引发异常

  ```go
  import "errors"
  
  func readConf(name string) (err error) {
      if name == "config.ini" {
          return nil
      } else {
          return errors.New("读取配置文件错误")
      }
  }
  
  func test() {
      err := readConf("config.ini")
      if err != nil {
          panic(err)
      }
  }
  ```

### 注意事项

* panic可以在任何位置引发，但是 recover 只能在 defer 中调用
* panic("message") 指定 panic 发生时显示的信息
