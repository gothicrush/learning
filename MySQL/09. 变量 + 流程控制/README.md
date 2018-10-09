## 变量

### 变量分类

* 系统变量
  * 全局变量：对于服务器所有的连接有效
  * 会话变量：只在当前连接有效
* 自定义变量
  * 用户变量：只在当前连接有效
  * 局部变量：仅在 BEGIN-END 中有效

### 系统变量

* 查看所有的系统变量

  ```mysql
  SHOW GLOBAL|SESSION VARIABLES;
  ```

* 查看某些的系统变量

  ```mysql
  SHOW GLOBAL|SESSION VARIABLES LIKE '%char%';
  ```

* 查看指定的系统变量

  ```mysql
  SELECT @@GOLBAL.系统变量名
  SELECT @@SESSION.系统变量名
  ```

* 修改系统变量的值

  ```mysql
  SET @@GLOBAL|SESSION.系统变量名 = 值
  ```

  

### 自定义变量

#### 用户变量

* 声明并赋值

  ```mysql
  SET @用户变量名 = 值
  SET @用户变量名 := 值
  SELECT @用户变量名 := 值
  ```

* 赋新值

  ```mysql
  SET @用户变量名 = 值
  SET @用户变量名 := 值
  SELECT @用户变量名 := 值
  
  SELECT 字段 INTO @变量名
  FROM 表;
  ```

* 使用

  ```mysql
  @变量名
  SELECT @变量名
  ```

#### 局部变量

* 声明并赋默认值

  ```mysql
  BEGIN
  
      DECLARE 局部变量名 变量类型 DEFAULT 默认值;
  
  END
  ```

  

## 流程控制

### 分支控制

#### 分类

* if
* case

#### if函数

* if(表达式1,表达式2,表达式3)
* 如果表达式1成立，则返回表达式2执行结果，否则返回执行表达式3执行结果
* 效果与三目运算符类似

#### case语句

* 类似于 switch 语句

  ```mysql
  CASE 变量|表达式|字段
  WHEN 要判断的值 THEN 返回的值1
  WHEN 要判断的值 THEN 返回的值2
  ...
  WHEN 要判断的值 THEN 返回的值n
  ELSE 要返回的值n+1;
  END CASE;
  ```

* 类似多重if语句

  ```mysql
  # 都不用加分号
  
  CASE
      WHEN 要判断的条件1 THEN 返回的值1
      WHEN 要判断的条件2 THEN 返回的值2
      ...
      WHEN 要判断的条件n THEN 返回的值n
      ELSE 要返回的值n+1
  END
  ```

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure5(IN target INT)
  BEGIN
      SELECT
          CASE
              WHEN target > 0 THEN 'greater than 0'
              WHEN target = 0 THEN 'equalse 0'
              ELSE 'less than 0'
          END     # 都不用加分号
      AS result;
  END;
  
  $
  
  DELIMITER ;
  ```

* 注意：ELSE可以省略，如果省略了且匹配不到，则返回NULL

### 循环控制

#### 使用场景

* 只能在函数或存储过程中使用

#### 分类

* WHILE
* LOOP
* REPEAT
* 循环控制语句
  * ITERATE：类似continue
  * LEAVE：类似break

#### WHILE

```mysql
【标签:】WHILE 循环条件 DO
    循环体
END WHILE【标签】;
```

#### LOOP

```mysql
【标签:】 LOOP
    循环体
END LOOP 【标签】;
```

#### REPEAT

```mysql
【标签:】REPEAT
    循环体
UNTIL 结束循环条件 END REPEAT【标签】;
```

