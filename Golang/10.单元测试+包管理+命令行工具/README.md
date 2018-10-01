# 单元测试

### 传统测试方法

* 在 main 函数中，调用需要测试的函数，看看实际结果与预期是否相同，如果相同，则正确，否则不正确
* 缺点：
  * 不方便，我们需要在 main 函数中调用，如果项目正在运行，则可能需要去停止项目
  * 不利于管理，不管什么模块的方法的测试都写在 main 函数里面了

### 单元测试框架

* Go中自带有一个轻量级测试框架 testing 和自带的 go test 命令来实现单元测试和性能测试

* 使用流程

  1. 创建测试文件

     ```go
     以 xxx_test.go 格式命名
     ```

  2. 导入测试框架

     ```go
     import "testing"
     ```

  3. 测试函数的签名

     ```go
     // 参数类型必须为 *testing.T（单元测试）或 *testing.B（性能测试）
     // 必须以Test开头，Test后的下一个字母必须是大写字母，否则单元测试不会执行这个函数
     func TestXxx(t *testing.T) { 
                                  
     }                            
     ```

  4. 编写测试函数

     ```go
     func TestXxx(t *testing.T) {
         t.SkipNow() // 跳过当前测试函数
         t.Fatalf("错误")
         t.Errorf("错误")
         t.Logf("正确")
     }
     ```

  5. 执行测试

     ```go
     go test // 所有测试文件都进行测试，如果运行正确，无日志，错误时，有日志
     go test -v // 所有测试文件都进行测试，无论运行是否正确，都有日志
     go test xxx_test.go // 指定测试文件
     go test -v xxx_test.go // 指定测试文件
     go test -v -test.run TestAdd // 只测试指定的函数
     ```

  6. 保证多个测试函数按顺序执行

     ```go
     func TestAll(t *testing.T) {      // 不一定是叫 TesttAll
         t.Run("TestPrint",TestPrint)  // TestPrint为函数名
         t.Run("TestPrint2",TestPrint2)
     }
     ```

  7. TestMain

     * 固定名字 
       * func (m testing.M) {}
     * 测试函数最开始执行这个函数，一般用于测试环境初始化
     * 如果 TestMain 中不写 m.Run()，则其他测试函数不会执行

### benchmark

* 用于性能测试

* 参数为(b *testing.B)，程序会执行 b.N 次，在执行过程中，会根据实际case的执行时间是否稳定改变b.N的值，以达到稳态 

* 例子

  ```go
  func BenchmarkTest(b *testing.B) {
      for n := 0; n < b.N; n++ {
          function()
      }
  }
  ```

* benchmark中的函数必须是稳定的，否则测试程序不能停止

  ```go
  func increse(n int) int {
      for n > 0 {
          n--
      }
      return n
  }
  
  func BenchmarkAll(b *testing.B) {
      for n := 0; n < b.N; n++ {
          increase(n) // 函数每次执行时间都不同
      }
  }
  ```
  
  
# go命令行工具
* go 在命令行输入 go，即可查看go命令行工具的说明
* go build 生成可执行二进制文件
* go clean 在go build后会残留一些临时文件，可以用go clean清除
* go run 执行go程序
* go install 类似go build，区别是生成的可执行二进制文件在指定位置
* go get 获取网上的 go 包
* go doc 在线获取包的文档
* go fmt 格式化代码
* go vet 检测代码中的语法错误
* go env 查看当前go环境


# 包管理

### 包的概述

* Go中每一个文件都属于一个包，即Go是以包的形式来管理文件和项目目录结构的
* 包的名字规范是全小写

### 包的作用

* 区分相同名字的标识符
* 控制函数，变量的作用域
* 对函数以及变量进行管理
* 声明某个go文件是某个包

### 声明包

```go
package db
```
### 导入包

* 从 src 目录起，写绝对路径

  ```go
  // 单个导入
  import "go_code/chapter03/demo04/model"
  
  // 批量导入
  import (
      "go_code/chapter03/demo04/model_1"
      "go_code/chapter03/demo04/model_2"
      "go_code/chapter03/demo04/model_3"
  )
  
  // 如果包太长，可以给包改名，也可以避免重名的问题
  import model "go_code/chapter03/demo04/model"
  ```

* 导入包可以写绝对路径和相对路径

  * 绝对路径：从 src 目录起，写绝对路径
  * 相对路径：以当前文件为基准点

### 包的搜索路径

* 标准库
* GOPATH指定目录/src

### 包的init函数

* 一个包一旦被导入，就会自动执行这个包中的init函数
* 一个包中可以有多个init函数
* 可以配合 _ 来使用

### 包的注意事项

* 文件名和包名最好相同

* 如果导入了包而不使用则会编译错误，如果只想执行包中的init函数而不想使用，则可以用 _

  ```go
  import _ "packagename"
  ```

* 如果想要定义一个go文件是可执行文件，则需要将这个文件所属包声明为main

  ```go
  package main // 这是规范
  ```

* 不能将多个包放在同一目录下，也不能将一个包拆分到多个目录下，总之一个包就是一个目录，这个目录名和包名相同

* 对于main包中不同的文件的代码不能互相调用
