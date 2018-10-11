### docker容器命令

#### 容器状态

* 容器标识
  * 容器长uuid
  * 容器短uuid
  * 容器名字

- 查看容器状态

  ```bash
  sudo docker status [容器标识1] [容器标识2] [容器标识n]
  ```

- 深入查看容器信息

  ```bash
  sudo docker inspect [容器标识]
  ```

- 查看docker中正在运行的容器

  ```bash
  sudo docker ps
  ```

- 查看docker中所有的容器，无论是否运行

  ```bash
  sudo docker ps -a
  ```

  

#### 容器创建

* 创建容器

  ```bash
  sudo docker create --name [容器名] [镜像名]
  ```

  * docker create：创建一个容器
  * --name [容器名]：为容器起名字为 [容器名]
    * 如果已存在则报错
  * [镜像名]：镜像的名字
    * docker会以这个镜像生成容器
    * 如果本地仓库不存在镜像，则docker会自动在远程库查找并下载



#### 容器启动

- 启动容器

  ```bash
  sudo docker start [容器标识]
  ```

* 创建并启动容器

  ```bash
  sudo docker run --name [容器名] -i -t [镜像名] [命令名]
  ```

  - docker run：创建并启动一个容器
  - --name [容器名]：为容器起名字为[容器名]
    - 如果名字已存在则报错，因为容器名要求唯一
  - -i -t：保证docker客户端能够与创建的容器以shell进行交互
    - -i：开启容器的stdin（标准输入）
    - -t：开启一个终端
  - [镜像名]：镜像的名字
    - docker会以这个镜像生成容器
    - 如果本地仓库不存在镜像，则docker会自动在远程库查找并下载
  - [命令名]：启动容器后执行什么命令

* 以守护进程方式启动容器

  ```bash
  sudo docker run --name [容器名] -i -t -d [镜像名] [命令名]
  ```

  * -d：以守护进程方式启动

* 自动重启容器式启动

  ```bash
  sudo docker run --restart=always --name [容器名字] [-d] [镜像名字] [命令]
  ```



#### 容器退出与停止

* 退出容器

  ```bash
  exit
  ```

* 停止守护容器

  ```bash
  sudo docker stop [容器标识]
  ```



#### 容器进程管理

* 查看容器中的进程

  ```bash
  sudo docker top [容器标识]
  ```

* 执行容器内部的程序

  ```bash
  sudo docker exec [-d] [容器标识] [进程命令]
  ```



#### 容器删除

* 删除容器

  ```bash
  sudo docker rm [容器标识]
  ```

  