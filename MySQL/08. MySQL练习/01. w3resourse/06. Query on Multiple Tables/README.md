1.
```sql
SELECT s.name AS salesman_name, c.cust_name AS customer_name, c.city AS city
FROM salesman AS s
JOIN customer AS c
ON s.city=c.city;
```

2.
```sql
SELECT c.cust_name AS customer_name,  s.name AS salesman_name
FROM customer AS c
JOIN salesman AS s
ON s.salesman_id = c.salesman_id;
```

3.
```sql
SELECT c.cust_name AS customer_name, c.city AS customer_city, s.name AS salesman_name, s.city AS salesman_city
FROM customer AS c
INNER JOIN salesman AS s
ON c.salesman_id = s.salesman_id
WHERE c.city != s.city;
```

4.
```sql
SELECT o.ord_no AS order_number, c.cust_name AS customer_name
FROM orders AS o
INNER JOIN customer AS c
ON o.customer_id = c.customer_id;
```

5.
```sql
SELECT customer.cust_name AS "Customer",
customer.grade AS "Grade"
FROM orders, salesman, customer
WHERE orders.customer_id = customer.customer_id
AND orders.salesman_id = salesman.salesman_id
AND salesman.city IS NOT NULL
AND customer.grade IS NOT NULL;
```

6.
```sql
SELECT c.cust_name AS customer_name, c.city AS customer_city, s.name AS salesman_name, s.commission AS salesman_commission
FROM customer AS c
INNER JOIN salesman AS s
ON c.salesman_id = s.salesman_id AND s.commission BETWEEN 0.12 AND 0.14;
```

7.
```sql
SELECT o.ord_no AS order_number, 
c.cust_name AS customer_name, 
s.commission AS commission_rate, 
o.purch_amt*s.commission AS commission
FROM orders AS o
INNER JOIN salesman AS s
ON o.salesman_id = s.salesman_id
INNER JOIN customer AS c
ON c.salesman_id = s.salesman_id
WHERE c.grade >= 200;
```
