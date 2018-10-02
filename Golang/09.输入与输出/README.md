# Part 6 - IO

### 从键盘获取数据

```go
import fmt

// fmt.Scanln() // 换行时停止扫描

var name string
var age byte
var sal float32
var isPass bool

fmt.Scanln(&name)
fmt.Scanln(&age)
fmt.Scanln(&sal)
fmt.Scanln(&isPass)


// fmt.Scanf()  // 扫描文本，根据format参数指定格式将读取空白分隔的值保存进本函数中
var name string
var age byte
var sal float32
var isPass bool

fmt.Scanf("%s %d %f %t", &name, &age, &sal, &isPass)
```



### 读取命令行参数

* 方式一：通过 os.Args

  ```go
  func main() {
      for index,val := range os.Args {
          fmt.Println(index, val)
      }
  }
  ```

* 方式二：通过 flag 包

  ```go
  import "flag"
  
  func main() {
      var user string
      var pwd string
      var host string
      var port int
      
      flag.StringVar(&user, "u", "","用户名，默认为空")
      flag.StringVar(&pwd, "pwd", "","密码，默认为空")
      flag.StringVar(&host, "h", "localhost", "主机名，默认为localhost")
      flag.StringVar(&port, "p", "3306", "端口号，默认为3306")
      
      flag.Parse()
  }
  ```

### 文件的输入流与输出流

#### 什么是输入流/输出流

* 文件在程序中是以**流**的形式进行操作的

* 输入流：数据从文件到内存
* 输出流：数据从内存到文件

#### 文件操作的包

* Go中文件操作的API都在 os 库中

#### 打开文件

```go
func os.Open(path string) (f *File, err error)
```

#### 关闭文件

```go
func (f *File) Close() error
配合 defer 语句使用
```

#### 读文件

* 带缓存读

```go
// 默认缓冲区大小为4096
reader := bufio.NewReader(file)

for {
    str,err := reader.ReadString('\n')
    
    if err == io.EOF {
        break
    }
}
```

* 一次性读取

```go
func ioutil.ReadFile(path string) (content []byte, err error) {}

file := "/home/test.text"
content, err := ioutil.ReadFile(file) // 不需要打开和关闭文件，这些操作都封装到ReadFile函数中了，适合小文件读取
if err != nil {
    fmt.Println(string(content))
}
```

#### 写文件

```go
func OpenFile(path string, flag int, perm FileMode) (file *File,err error)
```

* 模式 flag，可以通过 | 组合使用

```
O_RDONLY
O_WRONLY
O_RDWR
O_APPEND
O_CREATE
O_EXCL
O_SYNC
O_TRUNC
```

* 权限 perm

```go
r -> 4
w -> 2
x -> 1
```

* 案例

  * 创建一个新文件，写入5句 "Hello World"

    ```go
    file := "/home/abc.txt"
    file, err := os.OpenFile(file, os.O_WRONLY | os.O_CREATE, 0666)
    
    if err != nil {
        fmt.Println("open file successfully")
        return
    }
    
    defer file.Close()
    
    str := "hello, world"
    writer := bufio.NewWriter(file)
    
    for i := 0; i < 5; i++ {
        writer.WriterString(str)
    }
    
    // 因为带缓存，因此要刷新缓存
    writer.Flush()
    ```

  * 打开一个存在的文件，将原来的内容覆盖为新的内容，10句 "你好"

    ```go
    file := "/home/abc.txt"
    file, err := os.OpenFile(file, os.O_WRONLY | os.O_TRUNC, 0666)
    
    if err != nil {
        fmt.Println("open file successfully")
        return
    }
    
    defer file.Close()
    
    str := "你好你好"
    writer := bufio.NewWriter(file)
    
    for i := 0; i < 10; i++ {
        writer.WriterString(str)
    }
    
    // 因为带缓存，因此要刷新缓存
    writer.Flush()
    ```

  * 打开一个文件，在原来的内容基础上追加内容 " Append"

    ```go
    file := "/home/abc.txt"
    file, err := os.OpenFile(file, os.O_APPEND | os.O_TRUNC, 0666)
    
    if err != nil {
        fmt.Println("open file successfully")
        return
    }
    
    defer file.Close()
    
    str := " APPEND"
    writer := bufio.NewWriter(file)
    
    for i := 0; i < 10; i++ {
        writer.WriterString(str)
    }
    
    // 因为带缓存，因此要刷新缓存
    writer.Flush()
    ```

  * 打开一个存在的文件，将原来的内容读出显示在终端，并追加5句 "你好，世界"

    ```go
    file := "/home/abc.txt"
    file, err := os.OpenFile(file, os.O_RDWR | os.O_APPEND, 0666)
    
    if err != nil {
        fmt.Println("open file successfully")
        return
    }
    
    defer file.Close()
    
    reader := bufio.NewReader(file)
    for {
        str, err := reader.ReadString("\n")
        if err == io.EOF {
            break
        }
        fmt.Println(str)
    }
    
    str := "你好，世界"
    writer := bufio.NewWriter(file)
    
    for i := 0; i < 5 i++ {
        writer.WriterString(str)
    }
    
    // 因为带缓存，因此要刷新缓存
    writer.Flush()
    ```

  * 将一个文件的内容，写入到另外一个文件(注意：这两个文件已经存在了)

    ```go
    file1Path := "/home/abc.txt"
    file2Path := "/home/def.txt"
    
    data, err := ioutil.ReadFile(filePath)
    
    if err != nil {
        fmt.Println("文件读取失败")
        return
    }
    
    err = ioutil.Write(file2Path, data, 0666)
    
    if err != nil {
        fmt.Println(err)
    }
    ```

#### 判断文件与目录是否存在

```go
根据 os.Stat() 函数返回值进行判断
1. 如果返回值为nil，说明文件或目录存在
2. 如果返回值使用os.IsNotExist()判断为true，则说明文件或目录不存在
3. 如果返回值为其他类型，则不确定是否存在
```

```go
func PathExisting(path string) (bool,error) {
    _, err := os.Stat(path)
    if err == nil {
        return true, nil
    }
    
    if os.IsNotExist(err) {
        return false, nil
    }
    
    return false, err
}
```

#### 拷贝文件

```go
func io.Copy(dst Writer, src Reader) (Written int64, err error)
```

```go
func CopyFile(dstFilePath string, srcFilePath) (written int64, err error) {
    srcFile, err := os.Open(srcFilePath)
    
    if err != nil {
        fmt.Println("读取源文件错误")
        return
    }
    
    defer srcFile.Close()
    
    reader := bufio.NewReader(srcFile)
    
    dstFile, err := os.OpenFile(dstFilePath, os.O_WRONLY | os.O_CREATE, 0666) 
    
    if err != nil {
        fmt.Println("打开文件失败")
    }
    
    defer dstFile.Close()
    
    writer := bufio.NewWriter(dstFile)
    
    return io.Copy(writer, reader)
}
```
