# 类型断言

### 什么是类型断言

* 由于多态的存在，接口变量不知道其指向的具体类型，如果需要转为具体类型，则需要使用类型断言

### 类型断言语法

```go
接口变量名.(具体类型) // 此处变量必须为 interface 类型
```

### 类型断言返回值

```go
x := 变量名.(具体类型) // 如果转换成功则返回给x，转换失败则抛出 panic
x,ok := 变量名.(具体类型) // 如果转换成功则返回给x，ok为true，转换失败则 ok 为 false，x为类型默认值
```

### 类型断言例子

```go
type Point struct {
    x int
    y int
}

func main() {
    var a interface{}
    var point = Point{1,2}
    
    a = point
    
    var b Point
    
    // b = a //不行，虽然 a 指向的是Point类型，但是现在 a 是 Point 类型
    b = a.(Point) // 可以，这就是类型断言，表示判断 a 是否指向 Point 类型的变量
                  // 如果是则转为 Point 类型并赋值给 b 变量，否则抛出 panic
}
```

### 类型断言的最佳实践

```go
func checkType(items ...interface{}) {
    
    for index, x := range itmes {
        
        switch x.(type) { // 这里 type 是关键字，固定写法，只能用于 switch 语句
        case bool:
            fmt.Println("bool")
        case float64:
            fmt.Println("float64")
        case string:
            fmt.Println("string")
        case Student:
            fmt.Println("Student")
        case *Student:
            fmt.Println("*Student")
        }
    }
}
```

# JSON/序列化/反序列化

### JSON概念以及作用

* JSON全程 JavaScript Object Notation，是一种轻量级的数据交换格式，易于阅读和书写，也方便机器进行生成和解析
* 在数据传输前，传输的数据会经过处理变为 JSON 形式的字符串后再通过网络传输，接收方接收到 JSON 字符串后会进行处理，将 JSON字符串转为原始的数据
* 序列化：原始数据 -> JSON字符串
* 反序列化：JSON字符串 -> 原始数据 

### 序列化所需的包以及函数

```go
import "encoding/json"

func Marshal(v interface{}) ([]byte, error)
```

### 结构体进行序列化

* 将要进行序列化的结构体

  ```go
  type Monster struct {
      Name string
      Age int
      BirthDay string
      Sal float64
      Skill string
  }
  ```

* 结构体实例化实例

  ```go
  import "encoding/json"
  
  func main() {
      monster := {
          Name: "牛魔王",
          Age: 500,
          Birthday: "2011-11-11",
          Sal: 8000.0,
          Skill: "牛魔拳",
      }
      
      data,err := json.Marshal(monster)
      if err != nil {
          fmt.Println("序列化失败", err)
      } else {
          fmt.Println("序列化后：", string(data))
      }
  }
  ```

* 结构体使用tag自定义序列化后 key 值的名称

  ```go
  // `json:"xxx"`
  // 反引号是必须的
  type Monster struct {
      Name string `json:"name"`
      Age int     `json:"age"`
      BirthDay string `json:"birthday"`
      Sal float64 `json:"sal"`
      Skill string `json:"skill"`
  }
  ```

### map进行序列化

* 将要进行序列化的map

  ```go
  var m map[string]interface{}
  m["name"] = "红孩儿"
  m["age"] = 30
  m["address"] = "洪崖洞"
  ```

* map实例化实例

  ```go
  import "encoding/json"
  
  func main() {
      
      var m map[string]interface{}
      m["name"] = "红孩儿"
      m["age"] = 30
      m["address"] = "洪崖洞"
      
      data,err := json.Marshal(m)
      if err != nil {
          fmt.Println("序列化失败", err)
      } else {
          fmt.Println("序列化后：", string(data))
      }
  }
  ```

### 切片进行序列化

* 将要进行序列化的切片

  ```go
  var slice []int = []int{1,2,3,4,5}
  ```

* 切片实例化实例

  ```go
  import "encoding/json"
  
  func main() {
      
      var slice []int = []int{1,2,3,4,5}
      
      data,err := json.Marshal(slice)
      if err != nil {
          fmt.Println("序列化失败", err)
      } else {
          fmt.Println("序列化后：", string(data))
      }
  }
  ```

### 序列化所需的包以及函数

```go
import "encoding/json"

func Unmarshal(s []byte, v interface{}) (err error) 
```

### 反序列化为结构体

```go
package main

import "encoding/json"
import "fmt"

func main() {

    str := `{"address":"洪崖洞","age":30,"name":"红孩儿"}`

    var monster Monster

    err := json.Unmarshal([]byte(str), &m)

    if err != nil {
        fmt.Println("序列化失败", err)
    } else {
        fmt.Println("序列化后：", m)
    }
}

```

### 反序列化为map

```go
package main

import "encoding/json"
import "fmt"

func main() {

    str := `{"address":"洪崖洞","age":30,"name":"红孩儿"}`

    var m map[string]interface{}
    m = make(map[string]interface{})
    m["one"] = 1

    err := json.Unmarshal([]byte(str), &m)

    if err != nil {
        fmt.Println("序列化失败", err)
    } else {
        fmt.Println("序列化后：", m)
    }
}

```

### 反序列化为切片

```go
package main

import "encoding/json"
import "fmt"

func main() {

    str := `{"address":"洪崖洞","age":30,"name":"红孩儿"}`

    var m map[string]interface{}

    err := json.Unmarshal([]byte(str), &m)

    if err != nil {
        fmt.Println("序列化失败", err)
    } else {
        fmt.Println("序列化后：", m)
    }
}

```

### 注意事项

* 结构体在序列化时，非导出变量(小写字母开头)不会被 encode，因此在 decode 的时候这些非导出变量的值为其类型的零值

# 反射	

### 反射的概念与作用

* 反射可以在运行时动态获取变量的各种信息，比如变量的类型
* 如果变量是结构体，还可以获取结构体本身的信息，比如结构体的字段，方法
* 通过反射，可以修改变量的值，可以调用变量关联的方法

### 反射的使用

* 反射包

  ```go
  import "reflect"
  ```

### reflect.Value

* 由 reflect.ValueOf(v interface{}) (t reflect.Value) 获取某个变量的 Value
* reflect.Value.Kind：获取变量的类别，返回的是一个常量

* 变量，interface{}，reflect.Value 之间可以任意转换

  ```go
  // interface{} --> reflect.Value
  rVal := reflect.ValueOf(v)
  
  // reflect.Value --> interface{}
  iVal := rVal.Interface()
  
  // interface{} --> 变量 
  variable := iVal.(int64)
  
  // 变量 --> interface{}
  iVal = variable
  ```

  ![](/home/gothicrush/Desktop/golang/my_golang笔记/3.png)

### reflect.Type

* 由 reflect.TypeOf(v interface{}) (t reflect.Type) 获取某个变量的 Type
* reflect.Type.Kind：获取变量的类别，返回的是一个常量，与 reflect.Value.Kind 相同
* Type是类型，Kind是类别，它们可能相同，可能不同
  * var num int = 10  Type:int   Kind:int
  * var stu Student    Type:包名.Student  Kind:struct
