## 对于数据库的定义

### 创建库

```mysql
CREATE DATABASE IF NOT EXISTS 库名
DEFAULT CHARACTER SET utf8    //设置默认字符集为utf8
COLLATE uf8_general_ci;       //不区分大小写 case insensitive

CREATE DATABASE IF NOT EXISTS 库名
DEFAULT CHARACTER SET utf8    //设置默认字符集为utf8
COLLATE uf8_general_cs;       //区分大小写 case sensitive
```

### 修改库

* 直接修改文件夹的名称，不建议做

### 删除库

```mysql
DROP DATABASE IF EXISTS 库名;
```

### 更改库的字符集

```mysql
ALTER DATABASE 库名
CHARACTER SET gbk;
```





## 对于数据表的定义

### 创建表

```mysql
CREATE TABLE 表名 (
    字段名 字段类型【(长度)】【约束】,
    字段名 字段类型【(长度)】【约束】,
    ...
    字段名 字段类型【(长度)】【约束】,
    字段名 字段类型【(长度)】【约束】
);
```

### 修改表

* 修改字段名/类型/约束

  ```mysql
  ALTER TABLE 表名
  CHANGE COLUMN 旧字段名 新字段名 类型【(长度)】【约束】;
  ```

* 添加新字段

  ```mysql
  ALTER TABLE 表名
  ADD COLUMN 新字段名【(长度)】【约束】;
  ```

* 删除字段

  ```mysql
  ALTER TABLE 表名
  DROP COLUMN 字段名;
  ```

* 修改表名

  ```mysql
  ALTER TABLE 表名
  RENAME TO 新表名;
  ```

### 删除表

```mysql
DROP TABLE IF EXISTS 表名;
```

### 表的复制

* 仅仅复制结构

  ```mysql
  CREATE TABLE 表名
  LIKE 目标表名;
  ```

* 复制结构+数据

  ```mysql
  # 全部复制
  CREATE TABLE 表名
  SELECT * FROM 目标表名;
  
  # 部分复制
  CREATE TABLE 表名
  SELECT 部分字段 FROM 目标表名
  WHERE 筛选条件;
  ```

  

## 数据类型

### 数值

* 整数

  * 分类

    * Tinyint，1个字节
    * Smallint，2个字节
    * Mediumint，3个字节
    * Int，4个字节
    * Bigint，8个字节

  * 符号

    * 默认有符号
    * 设置无符号的方法，使用 UNSIGNED 关键字

    ```sql
    CREATE TABLE t (
        t1 INT,
        t2 INT UNSIGNED
    );
    ```

  * 长度

    * 长度表示最大显示宽度，当使用 zerofill 时，不够长度时，用0填空
    * 如果不配合zerofill，则整型的长度没用

  * 越界

    * 如果插入的数值超出范围，则报 out of range 异常，并插入最接近的临界值

* 浮点数

  * Float，4个字节
  * Double，8个字节

* 定点数

  * DECIMAL(M,D)
    * M：整数+小数的位数
    * D：小数的位数
    * M默认为10, D默认为0

### 字符型

* 较短文本
  * char：固定长度，性能高，不能节约空间
  * varchar：可变长度，性能低，能节约空间
  * 长度为最大长度
* 较长文本
  * text
* 二进制
  * binary：类似char，存储的是二进制
  * varbinary：类似varchar，存储的是二进制
  * Blob：存储数据量大的二进制，比如视频，图片

### 日期型

* date：2018-09-26
* time：17:11:37
* year：2018
* datetime：2018-09-26 17:11:50，与时区无关，8个字节
* timestamp：20180926171211，受MySQL版本影响，与时区有关，4个字节



## 数据约束

### 六大约束类型

* 非空约束：NOT NULL
* 默认约束：DEFAULT 值
* 唯一约束：UNIQUE 
* 主键约束：PRIMARY KEY
* 外键约束：FOREIGN KEY
* 检查约束：**MySQL不支持**

### 列级约束与表级约束

* 列级约束：约束语法都不报错，但是外键约束没有效果

* 表级约束：支持主键约束，外键约束，唯一约束

  ```mysql
  CREATE TABLE 表名(
      字段名 字段类型 列级约束
      字段名 字段类型
      CONSTRAINT 表级约束名 表级约束类型(字段名)
  )
  ```

* 列级约束例子

  ```mysql
  CREATE TABLE stuinfo (
      id      INT         PRIMARY KEY,                     # 主键约束
      stuName VARCHAR(20) NOT NULL,                        # 非空约束
      gender  CHAR(1)     DEFAULT 'm',                     # 默认约束
      seat    INT         UNIQUE,                          # 唯一约束
      major   INT         FOREIGN KEY REFERENCES major(id) # 外键约束，但是没有效果
  );
  ```

* 表级约束例子

  ```mysql
  CREATE TABLE stuinfo (
      id      INT         
      stuName VARCHAR(20) NOT NULL,                        # 非空约束
      gender  CHAR(1)     DEFAULT 'm',                     # 默认约束
      seat    INT,         
      major   INT,         
      
      CONSTRAINT pk PRIMARY KEY(id), # 主键约束
      CONSTRAINT uq UNIQUE(seat),    # 唯一约束
      CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id) # 外键约束
  );
  ```

### 主键约束和唯一约束的区别

| 约束类型 | 保证唯一性 | 是否允许为空 | 允许多少个 | 是否允许组合 |
| -------- | ---------- | ------------ | ---------- | ------------ |
| 主键     | 保证       | 不允许       | 最多1个    | 允许         |
| 唯一     | 保证       | 允许         | 可以多个   | 允许         |

### 外键使用注意事项

* 外键关联的必须是Key，一般是 主键/唯一键
* 插入数据时，先插入主表，再插入从表
* 删除数据时，先删除从表，再删除主表