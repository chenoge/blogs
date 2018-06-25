---
title: mysql-事务
date: 2018-04-14 20:11:32
tags: [mysql,事务]
---

# 事务控制语句

- **BEGIN**或**START TRANSACTION**；显式地开启一个事务；



- **COMMIT**或**COMMIT WORK**，COMMIT提交事务，并使已对数据库进行的所有修改称为永久性的；



- **ROLLBACK**或**ROLLBACK WORK**，结束用户的事务，并撤销正在进行的所有未提交的修改；



- **SAVEPOINT identifier**；SAVEPOINT允许在事务中创建一个保存点，一个事务中可以有多个SAVEPOINT；



- **RELEASE SAVEPOINT identifier**；删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；



- **ROLLBACK TO identifier**；把事务回滚到标记点；



- **SET TRANSACTION**；用来设置事务的隔离级别。InnoDB存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ和SERIALIZABLE。

<br/>

<!--more-->

```mysql
begin;  # 开始事务
insert into runoob_transaction_test value(5);
insert into runoob_transaction_test value(6);
commit; # 提交事务

begin;    # 开始事务
insert into runoob_transaction_test values(7);
rollback;   # 回滚
select * from runoob_transaction_test;   # 因为回滚所以数据没有插入
+------+
| id   |
+------+
| 5    |
| 6    |
+------+
```

