### 进入MySQL命令行

```sql
>> mysql -u 用户名 -p
>> 输入密码
```

### 查看MySQL的版本

```sql
-- 方法1：通过命令行
>> mysql --version
>> mysql -V

-- 方法2：通过MySql内置函数
>> SELECT VERSION();
```

### 查看所有的数据库

```sql
SHOW DATABASES;
```

### 打开指定的数据库

```sql
USE tables;
```

### 查看当前数据库所有的表

```sql
SHOW TABLES;
```

### 查看其他库所有的表

```sql
SHOW TABLES FROM 库名;
```

### 查看表结构

```sql
DESC 表名
```

### 执行 sql 脚本

```sql
SOURCE ./path/xxx.sql; -- 路径是相对路径；相对打开MySql时所处的路径
```

### 清空屏幕

```sql
Linux下：clear
Windows下：无法，可以exit -> cls -> 重新进入
```

### 退出MySql命令行

```sql
exit
```

