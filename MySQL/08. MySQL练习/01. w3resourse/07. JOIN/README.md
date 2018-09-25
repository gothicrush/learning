1.
```sql
SELECT s.name AS salesman_name, c.cust_name AS customer_name, s.city AS city
FROM salesman AS s
JOIN customer AS c
ON s.city = c.city;
```

2.
```sql
SELECT o.ord_no, o.purch_amt, c.cust_name, c.city
FROM orders AS o
JOIN customer AS c
ON c.customer_id = o.customer_id
WHERE o.purch_amt BETWEEN 500 AND 2000;
```

3.
```sql
SELECT c.cust_name AS customer_name , s.name AS salesman_name
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id;
```

4.
```sql
SELECT c.cust_name AS customer_name, s.name AS salesman_name, commission
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id
WHERE s.commission > 0.12;
```

5.
```sql
SELECT c.cust_name AS customer_name, s.name AS salesman_name, commission
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id AND c.city = s.city
WHERE s.commission > 0.12;
```

6.
```sql
SELECT 
    o.ord_no AS order_number,  
    o.ord_date AS order_date, 
    o.purch_amt AS purch_amt, 
    c.cust_name AS customer_name, 
    c.grade AS grade, 
    s.name AS salesman_name,
    s.commission AS commission
FROM orders AS o
INNER JOIN salesman AS s
ON o.salesman_id = s.salesman_id
INNER JOIN customer AS c
ON o.customer_id = c.customer_id;
```

7.
```sql
```

8.
```sql
SELECT c.cust_name AS customer_name, s.name AS salesman_name
FROM customer AS c
LEFT JOIN salesman AS s
ON c.salesman_id = s.salesman_id
ORDER BY s.salesman_id;
```

9.
```sql
SELECT c.cust_name AS customer_name, c.grade AS grade, s.name AS salesman_name
FROM customer AS c
LEFT JOIN salesman AS s
ON c.salesman_id = s.salesman_id
WHERE c.grade < 300;
```

10.
```sql

```

26.
```sql
SELECT *
FROM emp_department dp
JOIN emp_details de
ON dp.DPT_CODE = de.EMP_DEPT;
```

27.
```sql
```

28.
```sql

```

29.
```sql
SELECT d.DPT_NAME AS DPT_NAME, COUNT(*) AS EMP_NUMBER
FROM emp_department AS d
JOIN emp_details AS e
ON e.EMP_DEPT = d.DPT_CODE
GROUP BY d.DPT_CODE
HAVING COUNT(*) > 2;
```







