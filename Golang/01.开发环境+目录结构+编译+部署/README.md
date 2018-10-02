# 开发环境

### 环境变量设置

#### GOROOT

```go
指定 golang sdk 的安装目录
```

#### GOPATH

```go
golang 工作目录，项目的源码放在这个目录下
```

#### PATH

```go
将 GOROOT/bin 放在 Path 路径下，方便命令行能直接运行 golang的命令行工具
```



# 项目目录结构

```go
|--project                    // 位于GOPATH下
    |--src                    // 存放源代码
        |--packageA
            |--packageA.go
        |--packageB
            |--packageB.go
    |--pkg                    // 编译后生成的文件
    |--bin                    // 编译后生成的可执行文件
```


<<<<<<< HEAD
go build -o client.exe IM-CMD/client  // 在 src 下执行 go build
=======
go build -o client.exe IM-CMD/client  // 在 src 下执行 go build
>>>>>>> ad926c08f98d1bad4d0b2cbdd5f7b947cedfb3b0
