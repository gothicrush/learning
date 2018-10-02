### SQL慢的原因

- 查询语句写的烂
- 索引失效
- 太多 JOIN 使用
- MySQL参数设置有问题，比如缓冲太少等等

### SQL执行加载顺序

* 人写

```mysql
SELECT
FROM
JOIN ON
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

* 机读

```mysql
FROM
JOIN ON
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
LIMIT
```



### MySQL常见瓶颈

* CPU
* IO
* 硬件



### Explain进行性能分析

#### Explain是什么

* Explain是MySQL自带的SQL性能分析工具，能够知道MySQL是怎样处理SQL语句的 

#### Explain使用

* 语法

  ```mysql
  
  ```

* 例子

  ```mysql
  explain SELECT * FROM user;
  ```

#### Explain结果分析

| 字段          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| id            | id相同为一组，从上往下执行；id越大组的优先级越高             |
| select_type   | 查询的类型；<br />SIMPLE：简单查询, <br />PRIMARY：子查询中最外层查询,<br />SUBQUERY：子查询中内层查询,<br />DERIVED：产生的临时表,<br />UNION：联合查询,<br />UNION RESULT：联合查询的结果集 |
| table         | 数据来源的表                                                 |
| type          | 访问类型；<br />性能为：system > const > eq_ref > ref > range（至少） > index > ALL |
| possible_keys | SQL执行过程中可能使用到的索引                                |
| key           | SQL执行过程中实际使用到的索引                                |
| key_len       | SQL执行过程中实际使用到的索引占用的字节数<br />它是计算出来的，并非实际使用长度 |
| ref           | 显示索引中哪一列被使用了                                     |
| rows          | SQL执行过程中实际读取的行数                                  |
| Extra         | 包含十分重要，但不方便在表格中显示的内容<br />Using filesort<br />Using temporary<br />Using index<br />Using where<br />Using join buffer<br />impossible where<br />select tables optimized away<br />distinct |

