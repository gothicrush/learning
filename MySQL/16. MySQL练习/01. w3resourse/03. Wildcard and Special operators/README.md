网址：https://www.w3resource.com/sql-exercises/sql-wildcard-special-operators.php

1.
```sql
SELECT *
FROM salesman
WHERE city IN ('Paris', 'Rome');
```

2.
```sql
SELECT *
FROM salesman
WHERE city IN ('Paris', 'Rome');
```

3.
```sql
SELECT salesman_id,name,city,commission
FROM salesman
WHERE city != 'Paris' AND city != 'Rome';
```

4.
```sql
SELECT *
FROM customer
WHERE customer_id in (3007,3008,3009)
ORDER BY customer_id;
```

5.
```sql
SELECT *
FROM salesman
WHERE commission BETWEEN 0.12 AND 0.14;
```

6.
```sql
SELECT *
FROM orders
WHERE (purch_amt BETWEEN 500 AND 4000) AND (purch_amt NOT IN (948.50,1983.43));
```

7.【不懂不懂不懂】
```sql
SELECT *
FROM salesman
WHERE name BETWEEN 'A' and 'L';
```

8.
```sql
SELECT *
FROM salesman
WHERE name NOT BETWEEN 'A' and 'L';
```

9.
```sql
SELECT *
FROM customer
WHERE cust_name LIKE 'B%';
```

10.
```sql
SELECT *
FROM customer
WHERE cust_name LIKE '%n';
```

11.
```sql
SELECT *
FROM salesman
WHERE name LIKE 'N__l%';
```

12.
```sql
SELECT *
FROM testtable
WHERE col1 LIKE '%/_%' ESCAPE '/';
```

13.
```sql
SELECT *
FROM testtable
WHERE col1 NOT LIKE '%/_%' ESCAPE '/';
```

14.
```sql
SELECT *
FROM testtable
WHERE col1 LIKE '%//%' ESCAPE '/';
```

15.
```sql
SELECT *
FROM testtable
WHERE col1 NOT LIKE '%//%' ESCAPE '/';
```

16.
```sql
SELECT *
FROM testtable
WHERE col1 LIKE '%/_//%' ESCAPE '/';
```

17.
```sql
SELECT *
FROM testtable
WHERE col1 NOT LIKE '%/_//%' ESCAPE '/';
```

18.
```sql
SELECT *
FROM testtable
WHERE col1 LIKE '%/%%' ESCAPE '/';
```

19.
```sql
SELECT *
FROM testtable
WHERE col1 NOT LIKE '%/%%' ESCAPE '/';
```

20.
```sql
SELECT *
FROM customer
WHERE grade IS NULL;
```

21.
```sql
SELECT *
FROM customer
WHERE NOT grade IS NULL;
```

22.
```sql
SELECT *
FROM emp_details
WHERE EMP_LNAME LIKE 'D%';
```
