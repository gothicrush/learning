ELECT s.name AS salesman_name, c.cust_name AS customer_name, s.city AS city
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

17.
```sql

```

18.
```sql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL;
```

19.
```sql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL AND c.grade IS NOT NULL;
```

20.
```sql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL AND c.grade IS NOT NULL AND s.city != c.city;
```

21.
```sql
SELECT *
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM;
```

22.
```sql
SELECT im.PRO_NAME AS product_name, im.PRO_PRICE AS product_price, cm.COM_NAME AS company_name
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM;
```

23.
```sql
SELECT cm.COM_NAME AS company_name, AVG(im.PRO_PRICE) AS average_price
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM
GROUP BY cm.COM_NAME;
```

24.
```sql
SELECT cm.COM_NAME AS company_name, AVG(im.PRO_PRICE) AS average_price
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM
GROUP BY cm.COM_NAME
HAVING AVG(im.PRO_PRICE) >= 350;
```

25.
```sql
SELECT cm.COM_NAME AS company_name, IM.PRO_NAME AS product_name, im.PRO_PRICE AS product_price
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM
AND im.PRO_PRICE = (
    SELECT MAX(im.PRO_PRICE)
    FROM item_mast AS im
    WHERE im.PRO_COM = cm.COM_ID
);
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







