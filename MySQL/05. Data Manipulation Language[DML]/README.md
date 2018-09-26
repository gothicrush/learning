### INSERT INTO - 插入

* 语法1 - 【常用；支持多行；可用于子查询】

  ```sql
  INSERT INTO 表名(字段列表)
  VALUES(字段值列表),(字段值列表),(字段值列表),(字段值列表);
  ```

* 注意

  * 如果想设置空值，可以用NULL表示
  * 字段列表和字段值列表必须一一对应
  * 字段列表的顺序可以和表定义顺序不同
  * 可以省略某些字段

* 语法2 - 【不常用；不支持多行；不可用于子查询】

  ```sql
  INSERT INTO 表
  SET 字段名=值,字段名=值,...;
  ```

  

### UPDATE - 更新

* 语法1 - 【单表更新】

  ```sql
  UPDATE 表名 SET 字段名=值,字段名=值
  WHERE 筛选条件;
  ```

* 语法2 - 【多表更新】

  ```sql
  UPDATE 表1 AS 别名
  INNER | LEFT | RIGHT JOIN 表2 AS 别名
  ON 连接条件
  SET 字段名=值
  WHERE 筛选条件;
  ```

* 例子

  ```sql
  UPDATE boys AS b
  INNER JOIN girls AS g
  ON b.id = g.id
  SET g.phone = 1111
  WHERE b.name = "小明";
  ```

  

### DELETE FROM - 删除

* 语法1 - 【单表删除】

  ```sql
  DELETE FROM 表名
  WHERE 筛选条件;
  ```

* 语法2 - 【多表删除】

  ```sql
  DELETE FROM 表1 AS 别名
  INNER | LEFT | RIGHT JOIN 表2 AS 别名
  ON 连接条件
  WHERE 筛选条件;
  ```

* 例子

  ```sql
  DELETE FROM girls AS g
  INNER JOIN boys AS b
  ON b.id = g.id
  WHERE b.name="小明";
  ```

### TRUNCATE TABLE - 清空

* 语法

  ```sql
  TRUNCATE TABLE boys;
  ```

* 注意事项

  * DELETE FROM 是删除表中某些行数据，TRUNCATE TABLE是清空整张表
  * DELETE FROM删除后，自增字段不重置；TRUNCATE TABLE清空后，自增字段重置为1
  * DELETE FROM 可以回滚，TRUNCATE TABLE 不能回滚

