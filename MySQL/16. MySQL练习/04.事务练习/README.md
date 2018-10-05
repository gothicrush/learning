### 事务的概念

* 什么是事务

  ```mysql
  一条或一组SQL语句组成的执行的单元，满足事务的4个特性
  ```

* 事务的4个特点

  ```mysql
  A：原子性
  C：一致性
  I：隔离性
  D：持久性
  ```

* 事务并发问题

  ```mysql
  脏读：T1读了T2已经修改但未提交的数据
  不可重复读：T1读了一次数据，T2修改了数据，T1再读，数据不一样
  幻读：T1读了一些数据，T2删除或增加了数据，T1再读，发现读的数据少/多了
  ```

### 事务的设置

* MySQL引擎对事务的支持

  ```mysql
  INNODB：支持事务
  MyISAM：不支持事务
  Memory：不支持事务
  ```

* MySQL的事务隔离级别

  ```mysql
  读未提交：READ UNCOMMITTED
  读已提交：READ COMMITTED
  可重复读：REPEATABLE READ
  串行化：SERIALIZABLE
  ```

* 查看当前MySQL连接的事务隔离级别

  ```mysql
  SELECT @@tx_isolation
  ```

* 设置MySQL的事务隔离级别

  ```mysql
  SET GLOBAL|SESSION TRANSACTION ISOLATION LEVEL [4个级别中的一个]
  ```

### 事务的使用

* 关闭事务自动提交

  ```mysql
  SET AUTUCOMMIT = 0;
  ```

* 开启事务

  ```mysql
  START TRANSACTION;
  ```

* 提交事务

  ```mysql
  COMMIT;
  ```

* 回滚事务

  ```mysql
  ROLLBACK
  ```

* 使用保存点

  ```mysql
  SAVEPOINT a;
  ROLLBACK a;
  ```

  

