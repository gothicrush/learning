### 事务是什么

* 事务是指由一条或者多条SQL语句组成的单元，这个单元在执行过程中具有ACID四个特征

### 事务的特征(ACID)

#### A：原子性

* 事务是一个不可再拆分的最小工作单元
* 事务中的SQL要不都成功，要不都失败（回滚）

#### C：一致性

* 一致性是对数据可见性的约束，保证一个事务在执行操作过程中的数据中间状态对其他事务是不可见的，其他事务只能看到事务成功后的数据，或事务执行前的数据（回滚了）
* 一致性关心的是数据的可见性，而原子性关心的是事务执行的结果

#### I：隔离性

* 一个事务的执行不能被其他的事务干扰，即在并发过程中事务与事务之间是相互隔离，互不干扰的

#### D：持久性

* 一个事务一旦提交，它对数据库中的数据改变是永久的，不能再进行回滚

### 事务并发中的问题

#### 脏读

- 对于两个事务T1，T2；
- T1读取了已经被T2更新，但**T2还没提交**的数据

#### 不可重复读

- 对于两个事务T1，T2；
- T1先读取了一个数据，然后该数据被T2更新了，最后T1再读一次，得到的数据与**第一次读的数据不同**

#### 幻读

- 对于两个事务T1，T2；
- T1先读取了一个字段的数据，然后T2**插入或删除**了一些数据，最后T1再次读该字段的数据时，发现与**第一次读相比多了/少了**一些的数据
- 不可重复读强调的数据**本身的更新**，幻读强调的时数据的**新增或减少**



### MySQL引擎对事务的支持

- innodb支持事务
- myisam不支持事务
- memory不支持事务



### MySQL中事务的使用

#### 隐式事务

* MySQL的事务默认是隐式事务，事务自动开启和自动提交，即一条DML语句就是一个事务
* 比如 INSERT INTO，UPDATE，DELETE FROM

#### 显式事务

```mysql
SET AUTOCOMMIT = 0; # 关闭事务自动提交功能
START TRANSACTION; # 开启事务
    SQL语句1
    SQL语句2
    ...
    SQL语句n
COMMIT; # 提交事务

SET AUTOCOMMIT = 0; # 关闭事务自动提交功能
START TRANSACTION; # 开启事务
    SQL语句1
    SQL语句2
    ...
    SQL语句n
ROLLBACK; # 回滚事务
```

* SAVEPOINT的使用

  ```mysql
  SET AUTOCOMMIT = 0; # 关闭事务自动提交
  START TRANSACTION # 开启事务
  	SQL语句1
  	SAVEPOINT a; # 设置保存点
  	SQL语句2
      ROLLBACK TO a; # 回滚到保存点
      ...
      SQL语句n
  COMMIT;
  ```



### MySQL的四种事务隔离级别

#### 四种隔离级别列表

| 隔离级别                    | 译名     | 避免脏读 | 避免不可重复读 | 避免幻读 |
| --------------------------- | -------- | -------- | -------------- | -------- |
| READ UNCOMMITTED            | 读未提交 | ×        | ×              | ×        |
| READ COMMITTED              | 读已提交 | √        | ×              | ×        |
| REPEATABLE READ (mysql默认) | 可重复读 | √        | √              | ×        |
| SERIALIZABLE                | 串行化   | √        | √              | √        |

#### 查看当前隔离级别

```mysql
SELECT @@tx_isolation
```

#### 修改隔离级别

```mysql
SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL     READ UNCOMMITTED
SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL     READ COMMITTED
SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL     REPEATABLE READ
SET SESSION|GLOBAL TRANSACTION ISOLATION LEVEL     SERIALIZABLE
```

