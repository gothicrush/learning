### SQL注入

* SQL注入是什么

  ```mysql
  网络上存在场景：用户填写表单或url，发送查询给服务端，服务端根据用户传入的数据，执行SQL代码 
  
  SQL注入：用户故意传入恶意数据，使服务端执行恶意代码
  ```

* SQL注入例子

  ```mysql
  有后端代码
  
  SELECT COUNT(*)
  FROM Login
  WHERE user_name=`'` + userName + `'` AND password=`'` + password + `'`;
  
  其中userName和password为用户填写的数据
  
  如果用户传入
  账号：admin';-
  密码：随便
  
  则SQL语句拼接后为
  SELECT COUNT(*)
  FROM Login
  WHERE user_name='admin';-'` AND password=`'` + password + `'`;
  
  则后面密码验证部分被成为注释的一部分，即只要有这个用户名，就算查询成功，而不需要检验密码
  ```

* 防止SQL注入方法

  * 避免SQL语句用拼接实现