### 视图的概念

* 视图是什么？

  ```mysql
  一个虚拟的表
  ```

* 视图存储的是什么

  ```mysql
  存储的是SQL逻辑，在使用视图时，会动态执行SQL逻辑，生成临时的表
  ```

* 视图的优点

  ```bash
  保密性好
  简化操作
  ```

  

### 视图的语法

> 先按照DML练习到插入为止

* 为student创建视图view_student，只选取id，name和phone

  ```mysql
  CREATE VIEW view_student
  AS
  SELECT id,name,phone
  FROM student;
  ```

* 查看view_student的结构和创建语句

  ```mysql
  DESC view_student; # 结构
  SHOW CREATE VIEW view_student; # 创建语句
  ```

* 用视图查询id为3的phone

  ```mysql
  SELECT phone
  FROM view_student
  WHERE id=3;
  ```

* 修改视图为选取id，name，phone和gender

  ```mysql
  CREATE OR REPLACE VIEW view_student
  AS
  SELECT id,name,phone,gender
  FROM student;
  ```

  ```mysql
  ALTER VIEW view_student
  AS
  SELECT id,name,phone,gender
  FROM student;
  ```

* 删除view_student

  ```mysql
  DROP VIEW IF EXISTS view_student;
  ```

  