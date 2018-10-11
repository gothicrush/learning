### 使用Docker的先决条件

* 系统要求：64位操作系统
* 内核要求：Linux内核大于**3.8**且支持且开启**cgroup**和**namespace**功能



### Centos 7下载安装Docker

* 检测内核以及操作系统是否满足条件

  ```bash
  sudo uname -r # 查询内核的版本，需要大于3.8，操作系统需要是64位
  ```

* 卸载本机上的docker【如果有】

  ```bash
  sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
  ```

* 安装必备工具

  ```bash
  sudo yum install -y yum-utils device-mapper-persistent-data lvm2
  ```

* 添加yum软件源

  ```bash
  sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  ```

* 更新yum缓存

  ```bash
  sudo yum makecache fast
  ```

* 安装docker-ce

  ```bash
  sudo yum -y install docker-ce
  ```

* 启动docker后台服务

  ```bash
  sudo systemctl start docker
  ```

* 配置镜像

  ```bash
  sudo vim /etc/docker/daemon.json
  
  {
      "registry-mirrors":["http://hub-mirror.c.163.com"]    
  }
  ```

* 测试docker是否能用

  ```bash
  >> docker run hello-world
  ```

  