网址：https://www.w3resource.com/sql-exercises/sql-fromatting-output-exercises.php

1.
```sql
SELECT salesman_id, name, city, CONCAT(commission, '%') AS commission
FROM salesman;
```

2.
```sql
SELECT CONCAT('FOR ', ord_date, ' there are ', COUNT(*), ' orders')
FROM orders
GROUP BY ord_date;
```

3.
```sql
SELECT *
FROM orders
ORDER BY ord_no ASC;
```

4.
```sql
SELECT *
FROM orders
ORDER BY ord_no ASC;
```

5.
```sql
SELECT *
FROM orders
ORDER BY ord_date, purch_amt DESC;
```

6.
```sql
SELECT *
FROM customer
ORDER BY customer_id ASC;
```

7.
```sql
SELECT salesman_id, ord_date, MAX(purch_amt)
FROM orders
GROUP BY salesman_id, ord_date
ORDER BY salesman_id ASC, ord_date ASC;
```

8.
```sql
SELECT cust_name, city, grade
FROM customer
ORDER BY grade DESC;
```

9.
```sql
SELECT customer_id, COUNT(*), MAX(purch_amt)
FROM orders
GROUP BY customer_id
ORDER BY COUNT(*) DESC, MAX(purch_amt) DESC;
```

10.
```sql
SELECT ord_date, CONCAT(SUM(purch_amt) * 0.15, '%') AS percent
FROM orders
GROUP BY ord_date
ORDER BY ord_date ASC;
```
