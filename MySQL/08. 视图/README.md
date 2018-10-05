### 视图是什么

* 视图是一张虚拟的表，视图本质上保存的是SQL语句，而不是实际的数据
* 当使用视图时，视图会根据保存的SQL语句动态生成虚拟的数据表

### 视图的优点

* 保密性好
* 简化操作
* 修改限制

### 视图的语法

#### 创建视图

```mysql
CREATE VIEW IF NOT EXISTS 视图名
AS
查询语句
```

```mysql
CREATE VIEW myview
AS

SELECT last_name, department_name, job_title
FROM employees AS e
JOIN department AS d
ON e.department_id = d.department_id
JOIN jobs AS j
ON j.job_id = e.job_id;
```

#### 删除视图

```mysql
DROP VIEW IF EXISTS 视图名;
```

#### 修改视图

* 方式1

  ```mysql
  CREATE OR REPLACE VIEW 视图名
  AS
  查询语句
  ```

* 方式2

  ```mysql
  ALTER VIEW 视图名
  AS
  查询语句
  ```

#### 查看视图

```mysql
DESC 视图名;
SHOW CREATE VIEW 视图名;
```

#### 使用视图

- 和普通的表用法相同，不过视图中的数据一般不能修改

```mysql
SELECT *
FROM myview
WHERE last_name LIKE '%a%';
```

- 视图中的数据修改，生成该视图的表的数据也会更改，所以视图只能进行很简单的修改
