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
  
  CALL myprocedure3(@invalue,@outvalue);
  
  SELECT @outvalue;
  ```

* 创建一个INOUT参数存储过程：传递进来一个数，令其变为10倍

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
  
  CALL myprocedure5(-1);
  CALL myprocedure5(0);
  CALL myprocedure5(1);
  ```

* 创建一个IN参数和OUT参数存储过程：输入一个数，如果等于1显示'Hello'，等于2显示'World'，否则显示'Byebye'，结果同时保存到OUT参数中

  ```mysql
  DELIMITER $
  
  CREATE PROCEDURE myprocedure6(IN target INT,OUT result VARCHAR(20))
  BEGIN
      SELECT
          CASE target
              WHEN target = 1 THEN 'Hello'
              WHEN target = 2 THEN 'World'
              ELSE 'ByeBye'
          END
      INTO result;
  END;
  
  $
  
  DELIMITER ;
  
  CALL myprocedure6(0,@ret1);
  CALL myprocedure6(1,@ret2);
  CALL myprocedure6(2,@ret3);
  
  SELECT @ret1,@ret2,@ret3;
  ```

* 创建一个无参的存储过程：循环打印 myemployees 库中 employees 表中 employee_id = 168的first_name3次

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
  
  CALL myprocedure5();
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
      tt:LOOP
          SET times = times + 1;
          SET result = result * times;
          
          IF times > 10
              THEN LEAVE tt;
          END IF;
      END LOOP;
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

  

### 函数

* 函数是什么，与存储过程的区别

  ```mysql
  类似存储过程，是一组编译好的SQL语句
  ```

* 函数与存储过程的区别

  ```mysql
  函数必须有返回值和return语句，参数形式为  参数名  参数类型，取消了参数模式
  函数如果使用 SELECT，则必须配合INTO关键字，因为函数不允许出现结果集
  ```

* 函数优点是什么

  ```mysql
  增强代码复用性
  将执行过程封装起来，增强安全性
  因为是已经编译好的SQL语句，性能较高
  ```

* 创建函数的语法

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION 函数名(参数名 参数类型) RETURNS 返回值类型
  BEGIN
      函数体
  END;
  
  $
  
  DELIMITER ;
  ```

* 函数能不要返回值，不要return语句吗

  ```mysql
  不行，必须要有返回值和return语句
  ```

* 创建一个无参的函数：用于查找 myemployees 库中 employees 表中 employee_id = 168的first_name

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction1() RETURNS VARCHAR(30)
  BEGIN
      DECLARE ret varchar(30);
      
      SELECT first_name INTO ret
      FROM employees
      WHERE employee_id = 168;
      
      RETURN ret;
  END;
  
  $
  
  DELIMITER ;
  
  SELECT myfunction1();
  ```

* 创建一个含参函数：接收传递进来的参数，返回这个参数与"-myfunction2"的拼接结果

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction2(invar VARCHAR(30)) RETURNS VARCHAR(30)
  BEGIN
      RETURN CONCAT(invar,'-myfunction2');
  END;
  
  $
  
  DELIMITER ;
  
  SELECT myfunction2('abc');
  ```

* 创建一个含参函数：传递来一个变量 invalue = 10，返回 invalue * 2

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction3(invalue INT) RETURNS INT
  BEGIN
      RETURN invalue * 2;
  END;
  
  $
  
  DELIMITER ;
  
  SET @invalue = 10;
  
  SELECT myfunction3(@invalue);
  ```

* 创建一个含参函数：输入一个数，如果大于0返回'greater than 0'，小于零返回'less than 0'，等于0返回'equals 0'

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction4(invalue INT) RETURNS VARCHAR(30)
  BEGIN
      RETURN 
          CASE
              WHEN invalue > 0 THEN 'greater than 0'
              WHEN invalue = 0 THEN 'equals 0'
              WHEN invalue < 0 THEN 'less than 0'
          END
      ;
  END;
  
  $
  
  DELIMITER ;
  
  SELECT myfunction4(-1);
  SELECT myfunction4(0);
  SELECT myfunction4(1);
  ```

* 创建一个含参函数：输入一个数，如果等于1返回'Hello'，等于2返回'World'，否则返回'Byebye'

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction5(invalue INT) RETURNS VARCHAR(30)
  BEGIN
      RETURN 
      	CASE invalue
      		WHEN invalue = 1 THEN 'Hello'
      		WHEN invalue = 2 THEN 'World'
      		ELSE 'ByeBye'
          END
      ;
  END;
  
  $
  
  DELIMITER ;
  
  SELECT myfunction5(1);
  SELECT myfunction5(2);
  SELECT myfunction5(3);
  ```

* 创建一个无参函数：返回1+2+...+100的结果【使用while循环】

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction6() RETURNS INT
  BEGIN
      DECLARE times INT DEFAULT 0;
      DECLARE retsum INT DEFAULT 0;
      
      WHILE times < 101 DO
          SET retsum = retsum + times;
          SET times = times + 1;
      END WHILE;
      
      RETURN retsum;
  END;
  
  $
  
  DELIMITER ;
  
  SELECT myfunction6();
  ```

* 创建无参函数：返回1x2x3x...x10的结果【使用LOOP循环】

  ```mysql
  DELIMITER $
  
  CREATE FUNCTION myfunction7() RETURNS INT
  BEGIN
      DECLARE times INT DEFAULT 1;
      DECLARE retsum INT DEFAULT 1;
      
      tt:LOOP
          SET retsum = retsum * times;
          SET times = times + 1;
          
          IF times > 10
              THEN LEAVE;
          END IF;
      END LOOP;
      
      RETURN retsum;
  END;
  
  $
  
  DELIMITER ;
  ```

* 查看所有的函数

  ```mysql
  SHOW FUNCTION STATUS;
  ```

* 查看函数myfunction1的创建语句

  ```mysql
  SHOW CREATE FUNCTION myfunction1;
  ```

* 删除本次练习所创建的所有函数

  ```mysql
  DROP FUNCTION myfunction1;
  DROP FUNCTION myfunction2;
  DROP FUNCTION myfunction3;
  DROP FUNCTION myfunction4;
  DROP FUNCTION myfunction5;
  DROP FUNCTION myfunction6;
  DROP FUNCTION myfunction7;
  ```

  

  