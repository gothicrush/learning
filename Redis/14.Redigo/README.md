## Redigo

### 驱动下载

```go
go get github.com/garyburd/redigo/redis
```

### 获取redis服务器连接

```go
c, err := redis.Dial("tcp", "127.0.0.1:6379")

if err != nil {
    panic(err)
}

defer c.Close()
```

### 命令使用

```go
v, err := c.Do("SET","hello","world")

v, err = redis.String(c.Do("GET","hello"))
```



## 