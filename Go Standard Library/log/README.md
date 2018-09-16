### 作用

* 用于进行日志记录



### 导入

```go
import "log"
```



### 使用

* 创建一个日志器

  ```go
  // 写入位置，日志前缀，日志选项
  logegr := log.New(io.Writer,"prefix_",log.Ldata | log.Ltime)
  ```

* 输出一条日志

  ```go
  logger.Output("log log log")
  ```

* 格式化输出一条日志

  ```go
  logger.Printf("%d 号异常",6)
  ```

* 退出程序前输出日志

  ```go
  logger.Fatal("exit...")
  ```

* 抛出异常时输出日志

  ```go
  logger.Panic("panic...")
  ```

* 获取日志选项

  ```go
  logger.Flags()
  ```

* 设置日志选项

  ```go
  logger.SetFlags( log.Ldata )
  ```

* 获取日志前缀

  ```go
  
  ```

* 设置日志前缀

  ```go
  logger.SetPrefix()
  ```



### 日志选项

* Ldate：2009/01/03
* Ltime：01：23：23
* Lmicroseconds：01：23：23.123123 会覆盖Ltime
* Llongfile：a/b/c.go:23 完整路径和行号
* Lshortfile：c.go:23 最终路径和行号，覆盖Llongfile
* LstdFlags：Ldate | Ltime 日志器的日志选项

