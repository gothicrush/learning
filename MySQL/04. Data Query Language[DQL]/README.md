[]

### 练习用数据库导入

* 下载相关资源中的 `myemployess.sql`
* 执行 sql 脚本

```sql
source myemployees.sql;
```

### 基础查询

#### SELECT子句

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

* 查询常量值

  ```sql
  SELECT 100;
  ```

* 查询表达式

  ```sql
  SELECT 1+2;
  ```

* 查询函数

  ```sql
  SELECT version();
  ```

#### 字段起别名

```sql
方式一：SELECT last_name AS 姓 FROM employees;
方式二：SELECT last_name 姓 FROM employees;

特殊符号：SELECT last_name AS "OUT#PUT" FROM employees;
```

#### 查询结果去重

```sql
SELECT DISTINCT department_id FROM employees;
```

#### + 号的使用

```sql
MySQL中 + 仅仅只有运算的作用，没有拼接的作用
```

| 操作数A | 操作数B | 作用                                    |
| ------- | ------- | --------------------------------------- |
| 数值    | 数值    | 加法运算                                |
| 字符    | 数值    | 尝试将字符转换为数值，成功：继续做加法  |
| 字符    | 数值    | 尝试将字符转换为数值，失败：字符被当作0 |
| NULL    | 任意    | 只要有一个为 NULL，结果一定为 NULL      |

#### 拼接函数

```sql
SELECT CONCAT('a','b','c','d') AS 结果;
SELECT CONCAT(last_name, first_name) AS name FROM employee;
```

#### IFNULL函数

```sql
-- IFNULL(exp1, exp2)
-- exp1：将要进行判断的字段
-- exp2：替换的字段
-- 如果 exp1 为 NULL，则返回替换的字段

SELECT IFNULL(commission_pct, 0) AS "奖金率", commission_pct
FROM employees;
```

#### LENGTH函数

```sql
-- 求出字段的长度
SELECT LENGTH(last_name)
FROM employees;
```



### 条件查询

#### WHERE子句

```sql
SELECT 查询列表 
FROM 表名
WHERE 条件表达式;
```

#### 比较运算符

```sql
> < >= <= = !=(<>) 
```

#### 逻辑运算符

```sql
&&  ||  !
AND OR NOT
```

#### 模糊查询

##### 模糊查询关键字

| 模糊查询语句      | 注意事项                                                     |
| ----------------- | ------------------------------------------------------------ |
| LIKE              | 与通配符配合使用                                             |
| BETWEEN  x  AND y | 包含边界，等价于 >= x && <= y                                |
| IN                | IN ( 待选列表 )，待选列表中的元素类型要相同                  |
| IS NULL           | 不能用 = 判断是否是 NULL，只能用 IS 判断是否是 NULL，仅可以判断 NULL |

##### 通配符

```sql
% ：任意多个字符，包含0个
_ ：任意1个字符
```

##### 转义通配符

```sql
\_
\%
```



### 排序查询

#### ORDER BY子句

```sql
SELECT *
FROM employees
ORDER BY salary DESC; -- DESC：降序，ASC:升序，默认为 ASC
```

#### 多重排序标准

```sql
-- 先按 salary 进行升序排序，保证满足前提条件的情况下，按 employee_id 进行降序排序
SELECT *
FROM employees
ORDER BY salary ASC, employee_id DESC;
```

