网址：https://www.w3resource.com/sql-exercises/sql-aggregate-functions.php

1.
```sql 
SELECT SUM(purch_amt)
FROM orders;
```

2.
```sql
SELECT AVG(purch_amt)
FROM orders;
```

3.
```sql
SELECT COUNT (DISTINCT salesman_id)
FROM orders;
```

4.
```sql
SELECT COUNT (DISTINCT cust_name)
FROM customer;
```

5.
```sql
SELECT COUNT (grade)
FROM customer;
```

6.
```sql
SELECT MAX(purch_amt)
FROM orders;
```

7.
```sql
SELECT MIN(purch_amt)
FROM orders;
```

8.
```sql
SELECT city, MAX(grade)
FROM customer
GROUP BY city;
```

9.
```sql
SELECT customer_id, max(purch_amt)
FROM orders
GROUP BY customer_id
ORDER BY customer_id;
```

10.
```sql
SELECT customer_id,ord_date,MAX(purch_amt) 
FROM orders 
GROUP BY customer_id,ord_date;
```

11.
```sql
SELECT salesman_id, MAX(purch_amt)
FROM orders
WHERE ord_date = '2012-08-17'
GROUP BY salesman_id;
```

12.
```sql
SELECT customer_id,ord_date, MAX(purch_amt)
FROM orders
GROUP BY customer_id, ord_date
HAVING MAX(purch_amt) > 2000;
```

13.
```sql
SELECT customer_id, ord_date, MAX(purch_amt)
FROM orders
WHERE purch_amt BETWEEN 2000 AND 6000
GROUP BY customer_id, ord_date;
```

14.
```sql
SELECT customer_id, ord_date, MAX(purch_amt)
FROM orders
WHERE purch_amt IN (2000,3000,5760,6000)
GROUP BY customer_id, ord_date;
```

15.
```sql
SELECT customer_id, MAX(purch_amt)
FROM orders
WHERE customer_id BETWEEN 3002 AND 3007
GROUP BY customer_id;
```

16.
```sql
SELECT customer_id, MAX(purch_amt)
FROM orders
WHERE customer_id BETWEEN 3002 AND 3007
GROUP BY customer_id
HAVING MAX(purch_amt) > 1000;
```

17.
```sql
SELECT salesman_id, MAX(purch_amt)
FROM orders
WHERE salesman_id BETWEEN 5003 AND 5008
GROUP BY salesman_id;
```

18.
```sql
SELECT COUNT(*)
FROM orders
WHERE ord_date = '2012-08-17';
```

19.
```sql
SELECT COUNT(*)
FROM salesman
WHERE NOT city IS NULL;
```

20.
```sql
SELECT ord_date,salesman_id,COUNT(*)
FROM orders
GROUP BY ord_date, salesman_id;
```

21.
```sql
SELECT AVG(PRO_PRICE)
FROM item_mast;
```

22.
```sql
SELECT COUNT(*)
FROM item_mast
WHERE PRO_PRICE >= 350;
```

23.
```sql
SELECT PRO_COM, AVG(PRO_PRICE)
FROM item_mast
GROUP BY PRO_COM;
```

24.
```sql
SELECT SUM(DPT_ALLOTMENT)
FROM emp_department;
```

25.
```sql
SELECT EMP_DEPT,COUNT(*)
FROM emp_details
GROUP BY EMP_DEPT;
```
