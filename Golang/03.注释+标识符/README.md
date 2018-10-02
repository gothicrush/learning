# 注释

* 行注释

```go
// 行注释
```

* 块注释(多行注释)

```go
//块注释不可以嵌套

/*
Comment 1
Comment 2
Comment 3
...
Comment n
*/
```



# 标识符

* 规则

```go
只能由 数字，英文字母和_组成

不能以数字开头

单独的 _ ，是一个特殊符号，不能用作标识符

var _ int = 64 //error

不能用保留关键字，int，float64 等等不是保留关键字，但不推荐使用
```

* 命名规范

  ```go
  要有意义，要尽量简短
  
  包名：保持package的名字和目录的名字相同
  
  变量名，函数名，常量名：采用驼峰命名法
  ```

* 首字母大写是公有，首字母小写是私有



### 预定义标识符和保留关键字

- 程序声明：import、package
- 程序实体声明和定义：chan、const、func、interface、map、struct、type、var
- 程序流程控制：go、select、break、case、continue、default、defer、else、fallthrough、for、goto、if、range、return、switch
