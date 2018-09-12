### 练习用数据库导入

* 下载相关资源中的 `myemployess.sql`
* 执行 sql 脚本

```sql
source myemployees.sql;
```

### 基础查询

```sql
SELECT 查询字段 FROM 表名;

-- 查询字段包括：表的字段，常量值，表达式，函数
-- 查询的结果是一个虚拟的表
```

* 查询表中单个字段

  ```sql
  SELECT last_name FROM employees;
  ```

* 查询表中多个字段

  ```sql
  SELECT last_name, salary, email FROM employees;
  ```

* 查询表中所有字段

  ```sql
  SELECT * FROM employees;
  ```

  