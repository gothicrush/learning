### 目录

* [练习用数据库导入](###练习用数据库导入)
* [基础查询](###基础查询)
* [条件查询](###条件查询)
* [排序查](###排序查询)

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



### 常用函数

#### 字符函数

* LENGTH

```sql
-- 求出字节的个数
SELECT LENGTH(last_name)
FROM employees;
```

* CONCAT

```sql
SELECT CONCAT('a','b','c','d') AS 结果;
SELECT CONCAT(last_name, first_name) AS name FROM employee;
```

* UPPER

```sql
SELECT UPPER(last_name) FROM employees;
```

* LOWER

```sql
SELECT UPPER(last_name) FROM employees;
```

* SUBSTR

```sql
SELECT SUBSTR("123456789", 5) // 56789 索引从1开始
SELECT SUBSTR("123456789", 5, 2) // 567 从索引5开始，长度为2
```

* INSTR

```sql
SELECT INSTR("123456789", "567") // 返回第一次出现的索引 5
SELECT INSTR("123456789", "xxx") // 找不到返回0
```

* TRIM

```sql
SELECT TRIM("    xxx    ") // 去除前后空格
SELECT TRIM("a" FROM "aaaaaaaaaaaaaxxxaaaxxxaaaaaaaaa") // 返回 "xxxaaaxxx"
```

* LPAD

```sql
SELECT LPAD("abc", 15, "*") // "***************abc"
SELECT LPAD("abc", 2, "*") // "ab"
```

* RPAD

```sql
SELECT RPAD("abc", 15, "*") // "abc***************"
SELECT RPAD("abc", 2, "*") // "bc"
```

#### 数学函数

* ROUND

```sql
SELECT ROUND(4.56); // 5
SELECT ROUND(-1.56) // -2
SELECT ROUND(-1.567, 2) // -1.57 
```

* CELL

```sql
SELECT CELL(1.0001) // 2
SELECT CELL(-1.02)  // -1
SELECT CELL(1.00)   // 1
```

* FLOOR

```sql
SELECT FLOOR(1.0001) // 1
SELECT FLOOR(-9.8)   //-10
```

* TRUNCATE

```sql
SELECT TRUNCATE(1.699999,1);  // 1.6
```



#### 日期函数

* NOW

```sql
SELECT NOW();
```

* CURDATE

```sql
SELECT CURDATE();
```

* CURTIME

```sql
SELECT CURTIME();
```

* YEAR

```sql
SELECT YEAR(NOW());
SELECT YEAR("2018-9-14 08:23:57");
```

> MONTH，DAY，HOUR，MINUTE，SECOND 同上

* STR_TO_DATE

```sql
STR_TO_DATE("9-13-1999", "%m-%-%y")

%y 18 %Y 2018
%m 08 %c 8
%d 08
%H 24小时制
%h 12小时制
%i 35
%s 05
```

* DATE_FORMAT

```sql
DATE_FORMAT("2018/6/6","%Y年%m月%d日")
```

#### 流程控制语句

* IF

```sql
SELECT IF(10<5,'大','小')
```

* CASE

```sql
CASE (要判断的表达式，有就是switch，否则是 if-else)
WHEN 常量1 THEN 语句1
WHEN 常量2 THEN 语句2
...
ELSE 语句x
END
```

#### 其他函数

* IFNULL函数

```sql
-- IFNULL(exp1, exp2)
-- exp1：将要进行判断的字段
-- exp2：替换的字段
-- 如果 exp1 为 NULL，则返回替换的字段

SELECT IFNULL(commission_pct, 0) AS "奖金率", commission_pct
FROM employees;
```

#### 分组函数

* SUM：忽略 NULL
* AVG：忽略 NULL
* MAX：忽略 NULL
* MIN：忽略 NULL
* COUNT：忽略 NULL
* 注意事项
  * sum avg 可以处理数值
  * max，min，count可以处理任何类型
  * 分组函数都忽略 NULL
  * 可以和 distinct 配合实现去重
  * COUNT(*) ：统计行数，只要有不含 NULL 的，都算一行
  * COUNT(1)：统计行数，只要有不含 NULL 的，都算一行 
  * 和分组函数一同查询的字段要求是 group by 后的字段



### 分组查询

#### GROUP BY子句

```sql
SELECT
FROM
GROUP BY 
ORDER BY
```

