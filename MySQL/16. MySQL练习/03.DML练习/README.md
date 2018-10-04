### 建表

* 按照DDL练习，先创建student表和home表



### 插入【增】

* 向student表中同时插入三个新的字段【1,Alice,f,15353535353】【2,Bob,m,13646464646】【3,Jack,m,13745908686】

  ```mysql
  INSERT INTO student(id,name,gender,phone)
  VALUES(1,'Alice','f','15353535353'),(2,'Bob','m','13646464646'),(3,'Jack','m','13745908686');
  ```

  

* 在home表中插入字段【1,2,'China',13367809875】(in_province使用默认值)

  ```mysql
  INSERT INTO home(id,stu_id,address,phone)
  VALUES(1,2,'China',13367809875);
  ```

* 在home表中插入字段【2,1,'USA','n',15353556543】，使用SET语法，而不是VALUES语法

  ```mysql
  INSERT INTO home
  SET id=2,stu_id=1,address='USA',in_province='n',phone='15353556543';
  ```



### 改动【改】

* 更新student表中，id为3的手机为12345678901

  ```mysql
  UPDATE student
  SET phone='12345678901'
  WHERE id=3;
  ```

* 级联更新student表和home表中，性别为f的行的student.phone为66666666666

  ```mysql
  UPDATE student AS s
  INNER JOIN home AS h
  ON s.id = h.stu_id
  SET s.phone='66666666666'
  WHERE s.gender='f';
  ```

  

### 删除【删】

* 删除student表中id为3的行

  ```mysql
  DELETE FROM student
  WHERE id=3;
  ```



### 清空

* 清空表 home表

  ```mysql
  TRUNCATE TABLE home;
  ```

  