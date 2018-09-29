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