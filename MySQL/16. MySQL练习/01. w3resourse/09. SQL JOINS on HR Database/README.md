1.
```mysql
SELECT e.FIRST_NAME, e.LAST_NAME, e.CITY, , d.DEPARTMENT_NAME
FROM employees AS e
JOIN departments AS d
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
```

2.
```mysql
SELECT e.FIRST_NAME, e.LAST_NAME, l.CITY, l.STATE_PROVINCE, d.DEPARTMENT_NAME
FROM departments AS d
JOIN employees AS e
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
JOIN locations AS l
ON d.LOCATION_ID = l.LOCATION_ID;
```

3.
```mysql
SELECT e.FIRST_NAME, e.LAST_NAME, e.SALARY, j.GRADE_LEVEL
FROM employees AS e
JOIN job_grades AS j
ON e.SALARY BETWEEN j.LOWEST_SAL AND j.HIGHEST_SAL;
```

4.
```mysql
SELECT e.FIRST_NAME, e.LAST_NAME, d.DEPARTMENT_ID, d.DEPARTMENT_NAME
FROM employees AS e
JOIN departments AS d
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID 
WHERE e.DEPARTMENT_ID IN (40,80);
```

5.
```mysql
SELECT e.FIRST_NAME, e.LAST_NAME, e.DEPARTMENT_ID, l.CITY, l.STATE_PROVINCE
FROM departments AS d
JOIN employees AS e
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
JOIN locations AS l
ON d.LOCATION_ID = l.LOCATION_ID
WHERE e.FIRST_NAME LIKE '%z%';
```

6.
```mysql
SELECT d.DEPARTMENT_NAME, d.DEPARTMENT_ID
FROM departments AS d
LEFT JOIN employees AS e
ON d.DEPARTMENT_ID = e.DEPARTMENT_ID;
```

7.
```mysql
SELECT FIRST_NAME, LAST_NAME, SALARY
FROM employees
WHERE SALARY < (
    SELECT SALARY 
    FROM employees
    WHERE EMPLOYEE_ID = 182
);
```

8.
```mysql
SELECT e.FIRST_NAME AS employee_first_name, m.FIRST_NAME AS manager_first_name
FROM employees AS e
JOIN employees AS m
ON e.MANAGER_ID = m.EMPLOYEE_ID;
```

9.
```mysql
SELECT d.DEPARTMENT_NAME,l.CITY,l.STATE_PROVINCE 
FROM departments AS d
JOIN locations AS l
ON d.LOCATION_iD = l.LOCATION_ID;
```

10.
```mysql
SELECT e.FIRST_NAME AS first_name, e.LAST_NAME AS last_name, d.DEPARTMENT_ID AS department_id, d.DEPARTMENT_NAME AS department_name
FROM employees AS e
LEFT JOIN deparments AS d
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID;
```

11.
```mysql
SELECT e.FIRST_NAME AS first_name, e.LAST_NAME AS last_name, e.DEPARTMENT_ID AS department_number
FROM employees AS e
JOIN employees AS ee
ON e.DEPARTMENT_ID = ee.DEPARTMENT_ID
WHERE ee.LAST_NAME='Taylor';
```

12.
```mysql

```

13.
```mysql
```

14.
```mysql
```

15.
```mysql
SELECT d.DEPARTMENT_NAME AS department_name, AVG(e.SALARY) AS average_salary, COUNT(*) AS employees_number
FROM employees AS e
JOIN departments AS d
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
WHERE e.COMMISSION_PCT IS NOT NULL
GROUP BY d.DEPARTMENT_NAME;
```
不懂为什么下面的不行
```mysql
SELECT d.DEPARTMENT_NAME AS department_name, AVG(e.SALARY) AS average_salary, COUNT(*) AS employees_number
FROM employees AS e
JOIN departments AS d
ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
GROUP BY d.DEPARTMENT_NAME
HAVING e.COMMISSION_PCT IS NOT NULL;
```

16.
```mysql
SELECT CONCAT(e.FIRST_NAME,' ',e.LAST_NAME) AS full_name, j.JOB_TITLE AS job_title
FROM employees AS e
JOIN jobs AS j
ON e.JOB_ID = j.JOB_ID
WHERE e.DEPARTMENT_ID = 80;
```

17.
```mysql

```

18.
```mysql
SELECT d.DEPARTMENT_NAME AS department_name, CONCAT(e.FIRST_NAME, ' ', e.LAST_NAME) AS manager_name
FROM departments AS d
JOIN employees AS e
ON d.DEPARTMENT_ID = e.DEPARTMENT_ID
WHERE d.MANAGER_ID = e.EMPLOYEE_ID;
```

19.
```mysql
SELECT j.JOB_TITLE AS job_title, AVG(e.SALARY) AS average_salary
FROM employees AS e
JOIN jobs AS j
ON e.JOB_ID = j.JOB_ID
GROUP BY j.JOB_TITLE;
```

20.
```mysql
```

21.
```mysql
```

22.
```mysql
```

23.
```mysql
SELECT jh.EMPLOYEE_ID, j.JOB_TITLE, jh.END_DATE-jh.START_DATE AS days
FROM jobs AS j
JOIN job_history AS jh
ON j.JOB_ID = jh.JOB_ID
WHERE jh.DEPARTMENT_ID = 80;
```

24.
```mysql
```

25.
```mysql
```

26.
```mysql
```

27.
```mysql
```




