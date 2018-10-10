## 存储过程

### 存储过程的概念和优点

* 概念：存储过程是一组预先编译好的SQL语句的集合
* 类比：存储过程类似于 Go 中的函数
* 优点：提高代码重用性，简化操作，减少编译次数



### 创建存储过程

* 创建语法

  ```mysql
  DELIMITER $  # 不能加分号
  
  CREATE PROCEDURE 存储过程名(参数列表)
  BEGIN
      存储过程语句块
  END;
  
  $
  
  DELIMITER ;
  ```

* 参数形式

  ```mysql
  参数模式 参数名 参数类型
  ```

  | 参数形式 | 说明                       |
  | -------- | -------------------------- |
  | IN       | 作为输入                   |
  | OUT      | 作为输出                   |
  | INOUT    | 既可作为输入，又可作为输出 |

* 注意事项

  * 如果存储过程只有一句SQL，则可以省略BEGIN-END
  * 存储过程中的SQL都必须以分号结尾，所以存储过程结尾可以用 DELIMITER 重新设置

* 例子

  ```mysql
  DELIMITER $  # 不能加分号
  
  CREATE PROCEDURE myprocedure1()
  BEGIN
      INSERT INTO admin(username,passwd)
      VALUES ('jack','0000'),('lily','0000');
  END; 
  
  $
  
  DELIMITER ;
  ```

  ```mysql
  DELIMITER $ # 不能加分号
  
  CREATE PROCEDURE myprocedure2(IN name VARCHAR(20))
  BEGIN
      SELECT *
      FROM boys AS b
      RIGHT JOIN girls AS g
      ON b.id = g.id
      WHERE b.name = name;
  END; 
  
  $
  
  DELIMITER ;
  ```

  ```mysql
  DELIMITER $ # 不能加分号
  
  CREATE PROCEDURE myprocedure3(IN username VARCHAR(20), IN passwd VARCHAR(20))
  BEGIN
      DECLARE result VARCHAR(20) DEFAULT ''; # 声明局部变量
      
      SELECT COUNT(*) INTO result # 为局部变量赋值
      FROM admin
      WHERE admin.username = username AND admin.passwd = passwd;
      
      SELECT result; # 使用局部变量
  END; 
  
  $
  
  DELIMITER ;
  ```

  ```mysql
  DELIMITER $ # 不能加分号
  
  CREATE PROCEDURE myprocedure4(IN girlName VARCHAR(20), OUT boyName VARCHAR(20))
  BEGIN
      SELECT b.name INTO boyName
      FROM boys AS b
      JOIN girls AS g
      ON b.id = g.id
      WHERE g.name = girlName;
  END; 
  
  $
  
  DELIMITER ;
  ```

### 删除存储过程

```mysql
DROP PROCEDURE myprocedure1;
```

> 存储过程不能更改，如果想改变，可以先删除，再创建

### 查看存储过程创建

```mysql
SHOW CREATE PROCEDURE myprocedure1;
```

### 查看有哪些存储过程

```mysql
SHOW PROCEDURE STATUS;
```

### 调用存储过程

```mysql
CALL 存储过程名(实参列表);

CALL myprocedure1();
CALL myprocedure2("小明");
CALL myprocedure3("xiaoming","123456");
CALL myprocedure4("小昭");
```



## 函数

### 函数的概念和优点

- 概念：函数是一组预先编译好的SQL语句的集合
- 类比：存储过程类似于 Go 中的函数
- 优点：提高代码重用性，简化操作，减少编译次数

### 创建函数

- 创建语法

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
  BEGIN
      函数语句块
  END;
  
  $
  
  DELIMITER ;
  ```

- 参数形式

  ```mysql
  参数名 参数类型
  ```

- 注意事项

  - 如果存储过程只有一句SQL，则可以省略BEGIN-END
  - 存储过程中的SQL都必须以分号结尾，所以存储过程结尾可以用 DELIMITER 重新设置
  - 必须有且只能有一个返回值，且必须有return语句
  - 函数如果使用 SELECT，则必须配合INTO关键字，使用SELECT ... INTO ... 的结构，因为函数中不允许出现结果集
  - 返回类型如果是varchar必须带长度

### 删除函数

```mysql
DROP FUNCTION myfunction;
```

### 查看函数

```mysql
SHOW CREATE FUNCTION myfunction;
```

### 调用函数

```mysql
SELECT 函数名(参数列表);
```





