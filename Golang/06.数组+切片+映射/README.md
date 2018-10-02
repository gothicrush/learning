# 数组

### 什么是数组

* 数组是一种数据类型，属于值类型
* 数组可以存放多个同一类型数据

### 数组定义

```go
var 数组名 [数组大小]数据类型
var a [5]int
```

### 数组初始化的4种方式

```go
var numArray01 [3]int = [3]int{1,2,3}
var numArray02 = [3]int{1,2,3}
var numArray03 = [...]int{1,2,3}  // 长度自行推导
var numArray04 = [3]{0:1,2:3} // 指定下标
```

### 数组遍历

```go
// 方法1：
for i := 0; i < len(arr); i++ {
    fmt.Println(arr[i])
}

// 方法2：
for index, value := range arr {
    fmt.Println(index, " ", value)
}
```

### 数组对比

* 数组可以用 == 和 != 进行比较
* 但不能用 > < >= <= 等符号

### 数组相互赋值

* 当数据类型和数组长度相同时，两个数组之间可以相互赋值

### 数组注意事项

* 数组一旦定义后，长度不能改变，且数组中每个元素都有数据类型对应的默认值

* 可以通过数组名来获取数组第一个元素的地址，即 数组名 == &数组名[0] == 数组首地址

* 数组中各个元素的地址间隔由数组类型决定，比如 int64 -> 相隔8，int32 -> 相隔4

* 数组是值类型，作为参数时，是值传递，即在函数内修改数组，不会对原数组影响，除非使用指针

* 不能将 [3]int 类型的数组传递给 [4]int 的参数，它们被认为是不同类型；但是 [...]int{1,2,3} 可以传递给 [3]int 的参数

* 对于指向数组的指针，仍然可以用[index]进行索值

  ```go
  var arr [5]int{1,2,3,4,5}
  var p = &arr
  // p[3] == arr[3]
  ```

### 二维数组

* 声明

  ```go
  var 数组名 [大小][大小]类型
  ```

* 赋值

  ```go
  数组名[n][m] = 123
  
  没有赋值或初始化就是默认值
  ```

* 初始化

  ```go
  var 数组名 [大小][大小]类型 = [][大小][大小]类型{{初值},{初值},{初值}}
  var 数组名 [大小][大小]类型 = {{初值},{初值},{初值}}
  var 数组名 = [大小][大小]类型{{初值},{初值},{初值}}
  var 数组名 = [...][大小]类型{{初值},{初值},{初值}}
  ```

* 使用

  ```go
  fmr.Println(数组名[n][m]) 
  ```

* 遍历

  ```go
  // 方法1：
  for i := 0; i< len(arr); i++ {
      for j := 0; j < len(arr[i]); j++ {
          fmt.Println(arr[i][j])
      }
  }
  
  // 方法2：
  for i,v := range arr {
      for j,v2 := range v {
          fmt.Println(v2)
      }
  }
  ```

* 示意图

  ![](C:\Users\narli\Desktop\my_golang笔记\6.png)

# 切片

### 什么是切片

* 切片是引用类型，指向一个结构体，结构体包括一个数组的指针，切片大小，切片容量
* 切片的长度是可变的，因为切片底层是动态数组，所以切片的操作和数组很类似

### 定义切片

```go
var 切片名 []类型
var s []int
```

### 切片初始化

```go
// 如果没有给切片赋值，则是类型的默认值
// 如果切片没有初始化，也是可以使用的，这与 map 必须初始化后才能使用不同

// 方式1：直接初始化
var s []int = []int{1,3,5}

// 方式2：由已存在数组创建
var intArr [5]int = [...]int{1,2,3,4,5}
s := intArr[1:3] // [1,3)
// arr[0:end] 等价 arr[:end]
// arr[start:len(str)] 等价 arr[start:]
// arr[0:len(str)] 等价 arr[:]

// 方式3：通过 make 来创建切片
// 通过 make 方法创建的切片，其底层数组是 make 内部维护的，外部不可见
// 所以切片的值是默认值
var s []int = make([]int, 4) // 只指定了 length，则capacity == length
var s []int = make([]int, 4, 10) // []type, length, capacity 
```

### 切片遍历

```go
var arr [5]int = [...]int{10,20,30,40,50}
slice := arr[0:3]

// 方式1
for i := 0; i < len(slice); i++ {
    fmt.Println(slice[i])
}

// 方式2
for i, v := range slice {
    fmt.Println(i, v)
}
```

### 切片追加元素

```go
var slice []int = []int{1,2,3}
slice = append(slice,5,6,7) // 返回新的 slice 
slice = append(slice,slice) // 可以把slice追加给slice
```

### 切片append操作原理

- 切片append操作本质就是对数组进行扩容
- Go底层会创建一个新的数组newArr
- 将slice原来的元素拷贝到新的数组newArr中
- newArr是底层维护的，程序员不可见
- 然后创建一个新的sliceNew，sliceNew的ptr指向newArr
- 最后返回 sliceNew
- 当切片容量少于等于1000时，以2倍扩容，当大于1000时，以1.25倍扩容

### 切片拷贝操作

```go
copy(para1,para2) //para1 和 para2 都是切片类型，将para2的内容复制到para1
```

### 切片内存布局

* 切片是引用类型，切片名变量存储的是一个结构体的地址，即切片名是结构体的指针

* 结构体包含三个值：

  * 封装数组的地址

  * 切片的大小

  * 切片的容量

    ```go
    type slice struct {
        ptr *[2]int
        length int
        capacity int
    }
    ```



### 切片和字符串

* 字符串底层是 []byte，因此也可以切片

  ```go
  str := "hello world"
  slice := str[0:5]
  ```

  ![](/home/gothicrush/Desktop/golang/my_golang笔记/2.png)



* 字符串是不可变的

  ```go
  str[0] = 'z' // error
  
  // 正确
  var temp []byte = []byte(str) // 只能处理字母和数字，因为中文是3个字节
  temp[0] = 'z'               
  str = string(temp)
  
  // 如果想要处理中文，则转为rune即可
  var temp []rune = []rune(str)
  temp[0] = 'z'               
  str = string(temp)
  ```

### 基于原有切片定义新切片

```go
slice := []int{1, 2, 3, 4, 5}
slice1 := slice[:]
slice2 := slice[0:]
slice3 := slice[:5]
```

* 设置切片长度和容量一样的好处

```go
让新切片的长度和容量一样，这样我们在追加操作的时候就会生成新的底层数组，和原有数组分离，就不会因为共用底层数组而引起奇怪问题，因为共用数组的时候修改内容，会影响多个切片
```

### 空切片和nil切片

```go
nil切片: var slice []int
空切片:  slice := make([]int,0)
```

# 映射

### 映射类型声明的三种方式

* 仅仅进行声明

  ```go
  var 变量名 map[keyType]valueType
  ```

* 声明时直接初始化

  ```go
  var 变量名 map[string]int = make(map[string]int, 10) // 10可以省略
  var 变量名 map[string]int = make(map[string]int) // 10可以省略
  ```

* 声明时直接赋值

  ```go
  var 变量名 map[string]int = map[string]int{
      "one": 1,
      "two": 2,
  }
  ```

### 映射 key 和 value 的要求

- key 必须支持 == 运算
  - 一般用 string 和 数值 ，还可以用 bool，指针，channel
  - 不能用 slice，map和function
- value可以是string，数值，bool，struct，map 不能用slice，map和function

### 映射的赋值 [增，改]

* m["key"] = value
* 当m中本身没有 key 时，会新增
* 当m中本身有 key 时，会覆盖 
* 当空间不够时，会自动扩容

### 映射的删除  [删]

* delete(m, "one")
* 当删除的 key 不存在时，既不操作也不报错
* Go中没有方法可以一次性清除整个map，如果想，则可以遍历，或者让变量指向一个新的 map，让 gc 把原来那个删除了

### 映射的查找 [查]

* val := m["one"]
* 访问不存在的 key 值，返回类型默认类型，而不报错
* val, findRes := m["one"]
* 如果找到了 val 为 key 对应值，findRes为 false
* 如果找不到 val 为 类型默认值 ，findRes为true

### 映射的遍历

* 不能用 for 循环，因为映射是无序的

* 需要使用 for-range 循环，其中 k 和 v 是拷贝的

  ```go
  for k,v := range m {
      fmt.Printf("k=%v,v=%v",k,v)
  }
  ```

* golang中的map是无序的，每次遍历的结果都可能不一样

### 映射的排序

* golang中的map是无序的，每次遍历的结果都可能不一样

* golang中没有专门的方法针对map的key进行排序

* 如果想要对映射排序，则可以先拿出所有的key，将key进行排序，再取出value

  ```go
  var keys []int
  
  for key, _ := range m {
      keys = append(keys, key)
  }
  
  sort.Ints(key)
  ```

### 映射注意事项

- 声明是不会分配内存的，默认值为nil
- 需要使用make来初始化，分配内存后才能使用，make就是给map分配空间
- map的存储是无序的
- 使用内建函数 len 可以获取映射中键值对的个数
- 映射底层的数据结构是哈希表，所以无序
- new函数对于map的作用：p := new(map[string]int)，仅仅分配了字典类型本身（实际就是个指针包装）所需内存，并没有分配键值对存储的内存，因此无法正常使用
