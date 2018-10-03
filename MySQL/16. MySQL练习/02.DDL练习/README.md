## 数据库

### 创建库

* 创建新的数据库 dbone，默认字符集为 utf8，不区分大小写

  ```mysql
  CREATE DATABASE IF NOT EXISTS dbone
  DEFAULT CHARACTER SET utf8
  COLLATE uft8_general_ci;
  ```

* 创建新的数据库 dbtwo，默认字符集为 utf8，区分大小写

  ```mysql
  CREATE DATABASE IF NOT EXISTS dbtwo
  DEFAULT CHARACTER SET utf8
  COLLATE utf8_general_cs;
  ```

### 更改库的字符集

* 更改 dbtwo的字符集为 gbk

  ```mysql
  ALTER DATABASE dbtwo
  CHARACTER SET gbk;
  ```

### 删除库

* 删除数据库 dbone

  ```mysql
  DROP DATABASE IF EXISTS dbone;
  ```

* 删除数据库 dbtwo

  ```mysql
  DROP DATABASE IF EXISTS dbtwo;
  ```

  

## 数据表

### 创建表并约束

* 创建表 student，字段，类型，约束如下【都使用列级约束】，创建后查看结构

  | 字段   | 类型    | 长度 | 约束                  |
  | ------ | ------- | ---- | --------------------- |
  | id     | int     | 10   | 主键约束              |
  | name   | varchar | 20   | 非空约束              |
  | gender | char    | 1    | 默认约束；默认值为'm' |
  | phone  | char    | 11   | 唯一约束              |

  ```mysql
  CREATE TABLE IF NOT EXISTS student (
      id int(10) PRIMARY KEY,
      name varchar(20) NOT NULL,
      gender char(1) DEFAULT 'm',
      phone char(11) UNIQUE
  );
  
  DESC student;
  ```

  

* 创建表 home，字段，类型，约束如下【能表级就表级，不行才列级别】，创建后查看结构

  | 字段        | 类型    | 长度 | 约束                   |
  | ----------- | ------- | ---- | ---------------------- |
  | id          | int     | 10   | 主键约束               |
  | stu_id      | int     | 10   | 外键约束；student : id |
  | address     | varchar | 20   | 非空约束               |
  | in_province | char    | 1    | 默认约束；默认值为'y'  |
  | phone       | char    | 11   | 唯一约束               |

  ```mysql
  CREATE TABLE IF NOT EXISTS home (
      id int(10),
      stu_id int(10),
      address varchar(20) NOT NULL,
      in_province char(1) DEFAULT 'y',
      phone char(11),
      
      CONSTRAINT pk PRIMARY KEY(id),
      CONSTRAINT fk_student_id FOREIGN KEY(stu_id) REFERENCES student(id),
      CONSTRAINT uk UNIQUE(phone)
  );
  
  DESC home;
  ```

### 修改表

* student表中添加新的字段，查看表结构

  | 字段  | 类型    | 长度 | 约束 |
  | ----- | ------- | ---- | ---- |
  | hobby | varchar | 20   | 无   |

  ```mysql
  ALTER TABLE student
  ADD COLUMN hobby varchar(20) NOT NULL;
  
  DESC student;
  ```

* 修改 student表中hobby字段约束为非空约束，查看student表结构

  ```mysql
  ALTER TABLE student
  MODIFY COLUMN hobby varchar(20) NOT NULL;
  
  DESC student;
  ```

* 修改 home表中phone字段为无约束，查看home表结构

  ```mysql
  ALTER TABLE home
  MODIFY COLUMN phone VARCHAR(11);
  
  DESC home;
  ```

* 删除 student表中hobby字段，查看student结构

  ```mysql
  ALTER TABLE student
  DROP COLUMN hobby;
  
  DESC student;
  ```

* 修改student表名为 super_student，查看当前数据库的表

  ```mysql
  ALTER TABLE student
  RENAME TO super_student;
  
  SHOW TABLES;
  ```

### 复制表

* 复制 super_student的表结构，命名为 copy_student，查看结构和内容

  ```mysql
  CREATE TABLE IF NOT EXISTS copy_student
  LIKE super_student;
  
  DESC copy_student;
  ```

* 复制 home的表结构和内容，命名为 copy_home，查看结构和内容

  ```mysql
  CREATE TABLE IF NOT EXISTS copy_home
  SELECT * FROM home;
  ```

### 删除表

* 删除表 home

  ```mysql
  DROP TABLE IF EXISTS home;
  ```

* 删除表 super_student;

  ```mysql
  DROP TABLE IF EXISTS super_student;
  ```

* 删除表 copy_home

  ```mysql
  DROP TABLE IF EXISTS copy_home;
  ```

* 删除表 copy_student

  ```mysql
  DROP TABLE IF EXISTS copy_student;
  ```

  