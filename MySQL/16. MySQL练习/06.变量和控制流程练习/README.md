### 变量

* 变量的分类有哪些

  ```bash
  变量可以分为系统变量和自定义变量两种
  
  系统变量是MySQL系统定义好的变量，系统变量可以分为全局变量和会话变量，全局变量是指全部MySQL连接中都有效的变量，会话变量是指当前MySQL连接中才有效的变量
  
  自定义变量可以分为用户变量和局部变量，用户变量是当前MySQL连接有效的变量，局部变量是指在存储过程或函数中有效的变量
  ```

* 查看MySQL系统变量中全部的全局变量和会话变量

  ```mysql
  SHOW GLOBAL VARIABLES; 
  SHOW 【SESSION】 VARIABLES;
  ```

* 查看MySQL中系统变量中含'char'的全局变量和会话变量

  ```mysql
  SHOW GLOBAL VARIABLES LIKE '%char%';
  SHOW 【SESSION】 VARIABLES LIKE '%char%';
  ```

* 查看MySQL中全局和会话事务隔离级别变量

  ```mysql
  SELECT @@GLOBAL.tx_isolation;
  SELECT @@【SESSION】.tx_isolation;
  ```

* 修改MySQL中会话事务隔离级别变量为读未提交，全局隔离级别变量为序列化

  ```mysql
  SET @@GLOBAL.tx_isolation = READ UNCOMMITTED
  SET @@【SESSION】.tx_isolation = SERIALIZABLE
  ```

* 设置自定义用户变量 name=gothicrush

  ```mysql
  SET @name = 'gothicrush'
  ```

* 查看name

  ```mysql
  SELECT name;
  ```

* 将MySQL自带数据库myemployees中employee_id为3的员工的first_name赋值给name

  ```mysql
  USE myemployees;
  
  SELECT first_name INTO @name
  FROM employees
  WHERE employee_id = 168;
  ```

* 查看name

  ```mysql
  SELECT @name;
  ```

* 局部变量

  放到存储过程和函数中练习





### 流程控制

#### 分支

* 分支有哪几种方式

  ```mysql
  IF函数
  CASE 具体值匹配
  CASE 范围匹配
  ```

* 默写分支的几种方式
  ```mysql
  IF(A,B,C)
  
  CASE 变量|表达式
  WHEN 匹配值1 THEN 返回值1;
  WHEN 匹配值2 THEN 返回值2;
  ...
  WHEN 匹配值n THEN 返回值n;
  ELSE 返回值n+1;
  END CASE;
  
  CASE
  WHEN 匹配范围1 THEN 返回值1;
  WHEN 匹配范围2 THEN 返回值2;
  ...
  WHEN 匹配范围n THEN 返回值n;
  END CASE;
  ```

* 因为循环只能写在存储过程或者函数中，所以循环的具体练习放在之后练习 

#### 循环

* 循环有哪几种方式

  ```mysql
  WHILE 
  LOOP
  REPEAT
  ```

* 默写循环的几种方法

  ```mysql
  【标签】:WHILE 循环条件 DO
      循环体
  END WHILE【标签】;
  
  【标签】:LOOP
      死循环体
  END LOOP 【标签】;
  
  【标签】:REPEAT
      循环体
  UNTIL 循环结束条件
  END REPEAT【标签】;
  ```

* 因为循环只能写在存储过程或者函数中，所以循环的具体练习放在之后练习