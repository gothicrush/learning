### 作用

* 生成随机数

### 位置

* math/rand

### 使用

#### 随机种子

```go
import "math/rand"

// 设置随机种子
rand.Seed(int64)
// 常用写法
rand.Seed(time.Now().UnixNano())
// 不写随机种子，默认为
rand.Seed(1)
```



#### 生成随机整数

```go
// 不指定范围
rand.Int()    int    // 返回一个非负的伪随机int值
rand.Int31()  int32  // 返回一个int32类型的非负的31位伪随机数
rand.Int63()  int64  // 返回一个int64类型的非负的63位伪随机数
rand.Uint32() uint32 // 返回一个uint32类型的非负的32位伪随机数

// 指定范围
rand.Intn()   int     // 返回一个取值范围在[0,n)的伪随机int值，如果n<=0会panic
rand.Int31n() int32   // 返回一个取值范围在[0,n)的伪随机int32值，如果n<=0会panic
rand.Int63n() int64   // 返回一个取值范围在[0,n)的伪随机int64值，如果n<=0会panic
```



#### 生成随机浮点数

```go
rand.Float32() float // 返回一个取值范围在[0.0, 1.0)的伪随机float32值
rand.Float64() float // 返回一个取值范围在[0.0, 1.0)的伪随机float64值
```

