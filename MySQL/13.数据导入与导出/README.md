### 导入数据

* mysql命令导入

  ```mysql
  >> mysql -u 用户名 -p 密码 < /home/abc/源.sql
  ```

* source命令导入

  ```mysql
  mysql> source /home/abc/源.sql
  ```

  

### 导出数据

* mysqldump命令导出

  ```bash
  >> mysqldump -u 用户名 -p 密码 数据库名 表名 > 导出文件路径
  ```

* select ... into outfile命令导出

  ```mysql
  SELECT * 
  FROM 表名
  INTO OUTFILE 导出文件路径
  ```

  

