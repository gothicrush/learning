# 结构体

### 定义结构体

```go
type Cat struct {
    Name string
    Age int
    Color string
    Hobby []string
}
```

### 结构体实例化的4种方法

```go
// 方法1
var catInstance Cat // 该变量中的字段的值都是默认类型

// 方法2
var catInstance Cat = Cat{"小白", 2, "white", []string{"吃鱼","喝奶"}}
var catInstance Cat = Cat {
    Name: "小白",
    Age: 2,
    Color: "white",
    Hobby: []string{"吃鱼","喝奶"},
}

// 方法3
var catInstancePtr *Cat = new(Cat)
(*catInstancePtr).Name = "小白" //等价于 catInstancePtr.Name = "小白"

// 方法4
var catInstancePtr *Cat = &Cat{}
(*catInstancePtr).Name = "小白" //等价于 catInstancePtr.Name = "小白"
// 不能写 *catInstancePtr.Name = "小白" 因为 . 的优先级高于 *
```

### 结构体实例初始化问题

```go
// 如果没有给字段赋值，则默认为零值
// 引用类型是nil，即还没有分配空间，对于这样的字段，需要先make，才能使用

var cat1 Cat 
             
fmt.Println(cat1.Name, cat1.Age, cat1.Color) //ok
fmt.Println(cat1.Hobby) //error，需要初始化后才能使用
```

### 结构体与tag

* 在定义结构体时，可以给每个字段加上一个tag

  ```go
  type Student struct {
      Name string "tag-name"
      Age int "tag-age"
  }
  ```

* 结构体的tag可以通过反射机制获取，常见用于序列化和反序列化

### 匿名结构

```go
name := struct {
    字段名1 类型
    字段名2 类型
    字段名3 类型
} {
    字段名1:value,
    字段名2:value,
    字段名3:value
}
//注意匿名的东西都只能使用":="的形式，因为无法指定类型
```

### 匿名字段

```go
type Person struct {
    string
    int
}
a := Person{"narlinen",20}
```

### 工厂模式

* 结构体没有构造函数，通过工厂模式来解决

  ```go
  type student struct {
      Name string
      Age int
  }
  
  func New(name string, age int) *student {
      return &student {
          Name: name,
          Age: age,
      }
  }
  ```

### 结构体注意事项

* 结构体的所有字段在内存中是连续的
* 结构体没有构造函数，通过工厂模式创建实例
* 结构体是用户单独定义的类型，和其他类型进行转换时，需要具有完全相同的字段（名字以及数量，以及类型）
* 用type给已定义的结构体起别名时，编译器会认为这是新的数据类型，它们之间的赋值需要显式转换
* 就算字段内容完全相同，但只要struct名字不一样，则就不相等 
  * 即 type Duration int64 后，Duration和int64仍然是不同类型，需要强制转换 

# 方法

### 什么是方法

* 方法类似于函数，只不过方法可以进行绑定，方法可以绑定到指定的数据类型上，使该数据类型的实例都可以使用这个方法
* 方法不能独自调用，只能通过绑定的数据类型的实例进行调用
* 自定义类型都可以有方法，不仅仅是struct

### 方法的定义与使用

```go
type A struct {
    Name string
}

func (a A) test() {
    fmt.Println(a.Name)
}

func main() {
    a := A{Name:"111"}
    a.test()
}
```

### 可以添加方法的类型

* 任何的自定义类型都可以添加相应的方法

### 方法注意事项

* 接收者必须有一个显式的名字，这个名字必须在方法中被使用 

* 如果方法不需要使用接收者的值，可以用 **_** 替换它 

  ```go
  func (_ receiver_type) methodName(parameter_list) (return_value_list) { ... }
  ```

* 类型和作用在它上面定义的方法必须在同一个包里定义，这就是为什么不能在 int、float 或类似这些的类型上定义方法，可以为当前包中任何类型定义方法，必须是当前包

* Go中的toString()方法

  ```go
  func (a type) String() string { ... }
  ```

### 值接收者和指针接收这

* 对于普通函数，接收者为值类型时，不能将指针类型的数据直接传递，反之亦然
* 对于方法，接收者为值类型时，该类型的值类型或指针类型都可以直接调用，反之亦然
  * 接收者为值类型
    * 调用者为对应值类型：直接调用
    * 调用者为对应类型的指针类型：编译器底层自动解指针后再调用
  * 接收者为指针类型
    * 调用者为对应值类型：编译器底层自动取地址后再调用
    * 调用者为对应值类型的指针类型：直接调用
  * 无论调用者是值类型还是指针类型，实际调用类型还是方法定义的接收者类型
