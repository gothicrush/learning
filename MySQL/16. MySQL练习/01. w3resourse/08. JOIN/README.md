网址：https://www.w3resource.com/sql-exercises/sql-joins-exercises.php#SQLEDITOR

.
```mysql
SELECT s.name AS salesman_name, c.cust_name AS customer_name, s.city AS city
FROM salesman AS s
JOIN customer AS c
ON s.city = c.city;
```

2.
```mysql
SELECT o.ord_no, o.purch_amt, c.cust_name, c.city
FROM orders AS o
JOIN customer AS c
ON c.customer_id = o.customer_id
WHERE o.purch_amt BETWEEN 500 AND 2000;
```

3.
```mysql
SELECT c.cust_name AS customer_name , s.name AS salesman_name
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id;
```

4.
```mysql
SELECT c.cust_name AS customer_name, s.name AS salesman_name, commission
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id
WHERE s.commission > 0.12;
```

5.
```mysql
SELECT c.cust_name AS customer_name, s.name AS salesman_name, commission
FROM customer AS c
JOIN salesman AS s
ON c.salesman_id = s.salesman_id AND c.city = s.city
WHERE s.commission > 0.12;
```

6.
```mysql
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

7.[不懂]
```mysql
SELECT * 
FROM orders AS o
NATURAL JOIN customer AS c  
NATURAL JOIN salesman AS s;
```

8.
```mysql
SELECT c.cust_name AS customer_name, s.name AS salesman_name
FROM customer AS c
LEFT JOIN salesman AS s
ON c.salesman_id = s.salesman_id
ORDER BY s.salesman_id;
```

9.
```mysql
SELECT c.cust_name AS customer_name, c.grade AS grade, s.name AS salesman_name
FROM customer AS c
LEFT JOIN salesman AS s
ON c.salesman_id = s.salesman_id
WHERE c.grade < 300;
```

10.[题目看不懂]
```mysql
SELECT a.cust_name,a.city, b.ord_no,
b.ord_date,b.purch_amt AS "Order Amount" 
FROM customer a 
LEFT OUTER JOIN orders b 
ON a.customer_id=b.customer_id 
order by b.ord_date;
```

11.[题目看不懂]
```mysql
SELECT a.cust_name,a.city, b.ord_no,
b.ord_date,b.purch_amt AS "Order Amount", 
c.name,c.commission 
FROM customer a 
LEFT OUTER JOIN orders b 
ON a.customer_id=b.customer_id 
LEFT OUTER JOIN salesman c 
ON c.salesman_id=b.salesman_id;
```

12.[题目看不懂]
```mysql
SELECT a.cust_name,a.city,a.grade, 
b.name AS "Salesman", b.city 
FROM customer a 
RIGHT OUTER JOIN salesman b 
ON b.salesman_id=a.salesman_id 
ORDER BY b.salesman_id;
```

13.[题目看不懂]
```mysql
SELECT a.cust_name,a.city,a.grade, 
b.name AS "Salesman", 
c.ord_no, c.ord_date, c.purch_amt 
FROM customer a 
RIGHT OUTER JOIN salesman b 
ON b.salesman_id=a.salesman_id 
RIGHT OUTER JOIN orders c 
ON c.customer_id=a.customer_id;
```

14.[题目看不懂]
```mysql
SELECT a.cust_name,a.city,a.grade, 
b.name AS "Salesman", 
c.ord_no, c.ord_date, c.purch_amt 
FROM customer a 
RIGHT OUTER JOIN salesman b 
ON b.salesman_id=a.salesman_id 
LEFT OUTER JOIN orders c 
ON c.customer_id=a.customer_id 
WHERE c.purch_amt>=2000 
AND a.grade IS NOT NULL;
```

15.
```mysql
SELECT c.cust_name AS customer_name, c.city AS customer_city, o.ord_no AS order_no, o.ord_date AS order_date, o.purch_amt AS purch_amount
FROM customer AS c 
FULL JOIN orders AS o
ON c.customer_id = o.customer_id;
```

16.
```mysql
SELECT c.cust_name AS customer_name, c.city AS customer_city, o.ord_no AS order_no, o.ord_date AS order_date, o.purch_amt AS purch_amount
FROM customer AS c 
FULL JOIN orders AS o
ON c.customer_id = o.customer_id
WHERE c.grade IS NOT NULL;
```

17.
```mysql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c;
```

18.
```mysql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL;
```

19.
```mysql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL AND c.grade IS NOT NULL;
```

20.
```mysql
SELECT *
FROM salesman AS s
CROSS JOIN customer AS c
WHERE s.city IS NOT NULL AND c.grade IS NOT NULL AND s.city != c.city;
```

21.
```mysql
SELECT *
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM;
```

22.
```mysql
SELECT im.PRO_NAME AS product_name, im.PRO_PRICE AS product_price, cm.COM_NAME AS company_name
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM;
```

23.
```mysql
SELECT cm.COM_NAME AS company_name, AVG(im.PRO_PRICE) AS average_price
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM
GROUP BY cm.COM_NAME;
```

24.
```mysql
SELECT cm.COM_NAME AS company_name, AVG(im.PRO_PRICE) AS average_price
FROM company_mast AS cm
JOIN item_mast AS im
ON cm.COM_ID = im.PRO_COM
GROUP BY cm.COM_NAME
HAVING AVG(im.PRO_PRICE) >= 350;
```

25.
```mysql
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
```mysql
SELECT *
FROM emp_department dp
JOIN emp_details de
ON dp.DPT_CODE = de.EMP_DEPT;
```

27.
```mysql
SELECT e.EMP_FNAME AS first_name, e.EMP_LNAME AS last_name, d.DPT_NAME AS department_name, d.DPT_ALLOTMENT AS department_allotment
FROM emp_details AS e
JOIN emp_department AS d
ON e.EMP_DEPT = d.DPT_CODE;
```

28.
```mysql
SELECT e.EMP_FNAME AS first_name, e.EMP_LNAME AS last_name, d.DPT_NAME AS department_name, d.DPT_ALLOTMENT AS department_allotment
FROM emp_details AS e
JOIN emp_department AS d
ON e.EMP_DEPT = d.DPT_CODE
WHERE d.DPT_ALLOTMENT > 50000;
```

29.
```mysql
SELECT d.DPT_NAME AS DPT_NAME, COUNT(*) AS EMP_NUMBER
FROM emp_department AS d
JOIN emp_details AS e
ON e.EMP_DEPT = d.DPT_CODE
GROUP BY d.DPT_CODE
HAVING COUNT(*) > 2;
```