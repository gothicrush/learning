网址：https://www.w3resource.com/sql-exercises/sql-boolean-operators.php

1.
```sql
SELECT *
FROM customer
WHERE grade > 100;
```

2.
```sql
SELECT *
FROM customer
WHERE grade > 100 AND city='New York';
```

3.
```sql
SELECT *
FROM customer
WHERE grade > 100 OR city='New York';
```

4.
```sql
SELECT *
FROM customer
WHERE NOT grade > 100 OR city='New York';
```

5.
```sql
SELECT *
FROM customer
WHERE NOT grade > 100 AND NOT city='New York';
```

6.
```sql
SELECT * 
FROM  orders 
WHERE NOT ((ord_date ='2012-09-10' 
AND salesman_id>505) 
OR purch_amt>1000.00);
```

7.
```sql
SELECT salesman_id,city,commission
FROM salesman
WHERE commission > 0.10 AND commission < 0.12;
```

8.
```sql
SELECT *
FROM orders
WHERE purch_amt<200 OR NOT (ord_date>='2012-02-10' AND customer_id<3009);
```

9.
```sql
SELECT *
FROM orders
WHERE (ord_date <> '2012-08-17' OR NOT customer_id > 3005) AND (NOT purch_amt < 1000);
```

10.[不懂，不懂，不懂]
```sql
SELECT ord_no,purch_amt, 
(100*purch_amt)/6000 AS "Achieved %", 
(100*(6000-purch_amt)/6000) AS "Unachieved %" 
FROM  orders 
WHERE (100*purch_amt)/6000>50;
```

11.
```sql
SELECT *
FROM emp_details
WHERE EMP_LNAME IN ('Dosni','Mardy');
```

12.
```sql
SELECT *
FROM emp_details
WHERE EMP_DEPT IN (47,63);
```
