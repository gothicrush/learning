网址：https://www.w3resource.com/sql-exercises/sql-retrieve-from-table.php

1.
```sql
SELECT * 
FROM salesman;
```

2.
```sql
SELECT 'This is SQL Exercise, Practice and Solution';
```

3.
```sql
SELECT 1,2,3;
```

4.
```sql
SELECT 10+15;
```

5.
```sql
SELECT 10+15*4;
```

6.
```sql
SELECT name,commission
FROM salesman;
```

7.
```sql
SELECT ord_date,salesman_id,ord_no,purch_amt
FROM orders;
```

8.
```sql
SELECT DISTINCT salesman_id
FROM orders;
```

9.
```sql
SELECT name,city
FROM salesman
WHERE city='Paris';
```

10.
```sql
SELECT *
FROM customer
WHERE grade>200;
```

11.
```sql
SELECT ord_no,ord_date,purch_amt
FROM orders
WHERE salesman_id=5001;
```

12.
```sql
SELECT WINNER
FROM nobel_win
WHERE year=1970;
```

13.
```sql
SELECT WINNER
FROM nobel_win
WHERE year=1971 AND subject='Literature';
```

14.
```sql
SELECT year,subject
FROM nobel_win
WHERE WINNER='Dennis Gabor';
```

15.
```sql
SELECT winner
FROM nobel_win
WHERE year>1950 AND subject='Physics';
```

16.
```sql
SELECT year,subject,winner,country
FROM nobel_win
WHERE year BETWEEN 1965 AND 1975 AND subject='Chemistry';
```

17.
```sql
SELECT *
FROM nobel_win
WHERE YEAR>1972 AND CATEGORY='Prime Minister' AND WINNER IN ('Menachem Begin','Yitzhak Rabin');
```

18.
```sql
SELECT year,subject,winner,country
FROM nobel_win
WHERE winner LIKE 'Louis%';
```

19.
```sql
SELECT winner
FROM nobel_win
WHERE (subject='Physics' AND year=1970) OR (subject='Economics' AND year=1971);
```

20.
```sql
SELECT winner
FROM nobel_win
WHERE year=1970 AND subject NOT IN ('Physiology','Economics');
```

21.
```sql
SELECT winner
FROM nobel_win
WHERE (year<1971 AND subject='Physiology') OR (year>1974 AND subject='Peace');
```

22.
```sql
SELECT *
FROM nobel_win
WHERE winner='Johannes Georg Bednorz';
```

23.
```sql
SELECT *
FROM nobel_win
WHERE subject NOT LIKE 'P%'
ORDER BY year DESC, winner;
```

23.
```sql
SELECT *
FROM nobel_win
WHERE subject NOT LIKE 'P%'
ORDER BY year DESC, winner;
```

24.[不懂，不懂，不懂]
```sql
SELECT *
FROM nobel_win
WHERE year=1970 
ORDER BY
 CASE
    WHEN subject IN ('Economics','Chemistry') THEN 1
    ELSE 0
 END ASC,
 subject,
 winner;
```

25.
```sql
SELECT *
FROM item_mast
WHERE pro_price BETWEEN 200 AND 600;
```

26.
```sql
SELECT AVG(pro_price)
FROM item_mast
WHERE pro_com=16;
```

27.
```sql
SELECT PRO_NAME,PRO_PRICE
FROM item_mast;
```

28.
```sql
SELECT PRO_NAME as name,PRO_PRICE as price
FROM item_mast
WHERE PRO_PRICE>=250
ORDER BY PRO_PRICE DESC,PRO_NAME ASC;
```

29.
```sql
SELECT AVG(PRO_PRICE),PRO_COM
FROM item_mast
GROUP BY PRO_COM;
```

30.
```sql
SELECT PRO_NAME,PRO_PRICE
FROM item_mast
WHERE PRO_PRICE = (
    SELECT MIN(PRO_PRICE)
    FROM item_mast
);
```

31.
```sql
SELECT DISTINCT EMP_LNAME
FROM emp_details;
```

32.
```sql
SELECT *
FROM emp_details
WHERE EMP_LNAME='Snares';
```

33.
```sql
SELECT *
FROM emp_details
WHERE EMP_DEPT=57;
```
