### 存储过程

* 存储过程是什么

  ```mysql
  存储过程是一组已经编译好的SQL语句
  ```

* 存储过程优点有什么

  ```mysql
  安全
  性能高
  提高代码复用性
  ```

* 创建存储过程的语法

  ```mysql
  DELIMITER $ # 不能加分号
  
  CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名 参数类型)
  BEGIN
      存储过程语句块
  END;
  
  $
  
  DELIMITER ;
  ```

* 创建一个无参的存储过程：打印 myemployees 库中 employees 表中 employee_id = 168的first_name，并调用

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure1()
  BEGIN
       SELECT first_name
       FROM employees
       WHERE employee_id = 168;
  END;
  
  $
  
  DELIMITER ;
  
  CALL myprocedure1();
  ```

* 创建一个IN参数存储过程：打印传递进来的参数

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure2(IN v INT)
  BEGIN
      SELECT v;
  END;
  
  $
  
  DELIMITER ;
  
  SET @v = 10;
  
  CALL myprocedure2(@v);
  ```

* 创建一个IN参数和OUT参数存储过程：传递来一个变量 invalue = 10，输出一个变量 outvalue = invalue * 2

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure3(IN invalue INT,OUT outvalue INT)
  BEGIN
      SELECT invalue * 2 INTO outvalue;
  END;
  
  $
  
  DELIMITER ;
  
  SET @invalue = 10;
  SET @outvalue = 0;
  
  CALL myprocedure3(@invalue,@outvalue)
  
  SELECT @outvalue;
  ```

* 创建一个INOUT参数存储 过程：传递进来一个数，令其变为10倍

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure4(INOUT inoutvalue INT)
  BEGIN
      SET inoutvalue = inoutvalue * 10;
  END;
  
  $
  
  DELIMITER ;
  
  SET @value = 10;
  
  CALL myprocedure4(@value);
  
  SELECT @value;
  ```

* 创建一个IN参数存储过程：输入一个数，如果大于0显示'greater than 0'，小于零显示'less than 0'，等于0显示'equals 0'

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure5(IN target INT)
  BEGIN
      SELECT
          CASE
              WHEN target > 0 THEN 'greater than 0'
              WHEN target = 0 THEN 'equalse 0'
              ELSE 'less than 0'
          END
      AS result;
  END;
  
  $
  
  DELIMITER ;
  ```

* 创建一个IN参数和OUT参数存储过程：输入一个数，如果等于1显示'Hello'，等于2显示'World'，否则显示'Byebye'，结果同时保存到OUT参数中

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure6(IN target INT,OUT result VARCHAR(20))
  BEGIN
      CASE target
          WHEN 
      END
  END;
  $
  
  DELIMITER ;
  ```

  

* 创建一个无参的存储过程：循环打印10次 myemployees 库中 employees 表中 employee_id = 168的first_name3次

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure5()
  BEGIN
      DECLARE times INT DEFAULT 0;
      WHILE times < 3 DO
          SELECT first_name
          FROM employees
          WHERE employee_id = 168;
          
          SET times = times + 1;
     END WHILE;
  END;
  
  $
  
  DELIMITER ;
  ```

* 创建一个IN参数存储过程：输入一个正整数，打印它这么多次"This is REPEAT UNITL"

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure6(IN times INT)
  BEGIN
      REPEAT 
          SELECT 'This is REPEAT UNTIL';
          SET times = times - 1;
      UNTIL times < 0 END REPEAT;
  END;
  
  $
  
  DELIMITER ;
  
  CALL procedure6(10);
  ```

* 创建一个OUT参数存储过程：返回1x2x3x...x10的结果给一个OUT参数result，使用LOOP循环

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure7(OUT result INT)
  BEGIN
      DECLARE times INT DEFAULT 1;
      SET result = 1;
      o:LOOP
          SET times = times + 1;
          SET result = result * times;
          
          CASE times
          WHEN times = 9 THEN LEAVE 0;
          END CASE;
  END;
  
  $
  
  DELIMITER ;
  
  SET @result = 0;
  
  CALL myprocedure7(@result);
  
  SELECT @result;
  ```

  

* 查看有哪些存储过程

  ```mysql
  SHOW PROCEDURE STATUS;
  ```

* 查看存储过程myprocedure1的创建语句

  ```mysql
  SHOW CREATE PROCEDURE myprocedure1;
  ```

* 删除当前数据库中本次练习创建的存储过程

  ```mysql
  DROP PROCEDURE myprocedure1;
  DROP PROCEDURE myprocedure2;
  DROP PROCEDURE myprocedure3;
  DROP PROCEDURE myprocedure4;
  DROP PROCEDURE myprocedure5;
  DROP PROCEDURE myprocedure6;
  DROP PROCEDURE myprocedure7;
  DROP PROCEDURE myprocedure8;
  DROP PROCEDURE myprocedure9;
  DROP PROCEDURE myprocedure10;
  ```

  