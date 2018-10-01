# Part 1 - 下载

* 访问https://www.mysql.com/
* 根据下图进行下载


# Part 2 - 安装 [Centos 7]

### 检查是否已经安装有Mariadb/MySQL
```
sudo rpm -qa|grep mariadb 
sudo rpm -qa|grep mysql
```

### 卸载已经安装的Mariadb/MySQL[如果有]
```
sudo rpm -e --nodeps 文件名 #文件名为上述命令查出来的名字
```

### 删除/etc/下的my.cnf[如果有]
```
sudo rm -rf /etc/my.cnf
```

### 创建mysql用户组和用户
```
sudo groupadd mysql
sudo useradd -g mysql mysql
```

### 解压并移动解压后文件到/usr/local
```
tar -zxvf mysql-5.7.tar.gz
mv mysql /usr/local
```

### 在/etc/下创建配置文件my.cnf
```
sudo touch /etc/my.cnf
```

### 配置my.cnf
```
sudo vim /etc/my.cnf
```
* 输入以下内容
```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8 
socket=/var/lib/mysql/mysql.sock

[mysqld]
skip-name-resolve
#设置3306端口
port = 3306 
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装目录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql/data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
lower_case_table_name=1
max_allowed_packet=16M
```

### 进行MySQL安装
```
sudo yum -y install autoconf
sudo cd /usr/local/mysql
sudo chown -R mysql:mysql
sudo /usr/local/mysql/script/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data/
```

### 设置MySQL自启动
```
sudo /usr/local/mysql/.support-files/mysql.server /etc/rc.d/init.d/mysqld
sudo chmod +x /etc/rc.d/init.d/mysqld
sudo chkconfig --add mysqld
```
* 检查是否成功
```
sudo chkconfig --list mysqld
```

### MySQL启动与停止与重启
```
启动：sudo service mysql start
停止：sudo service mysql stop
重启：sudo service mysql restart
```

### 将mysql加入环境变量
```
PATH="$PATH:/usr/local/mysql/bin"
```

### 进入MySQL并设置密码
```
sudo mysql
mysql> use mysql;
mysql> UPDATE user SET password=PASSWORD("新密码") WHERE user='root' AND host='localhost';
```
* 重启MySQL服务
```
sudo service mysql restart
```
