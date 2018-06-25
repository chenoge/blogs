---
title: mysql-where比较运算
date: 2018-04-14 16:19:03
tags: [mysql,where,运算符,通配符]
---

# 一、运算符

| 运算符          | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| =               | 等于                                                         |
| <>              | 不等于。**注释：**在 SQL 的一些版本中，该操作符可被写成 !=   |
| >               | 大于                                                         |
| <               | 小于                                                         |
| >=              | 大于等于                                                     |
| <=              | 小于等于                                                     |
| **And**         | 同时满足两个条件的值                                         |
| **Or**          | 满足其中一个条件的值                                         |
| **Not**         | 满足不包含该条件的值                                         |
| **is null**     | **空值判断**                                                 |
| **between and** | 在某个范围内                                                 |
| **LIKE**        | 模糊查询（  `%` 表示多个字值，`_ `**下划线**表示一个字符； ） |
| **IN**          | 指定针对某个列的多个可能值                                   |

<br/>

<!--more-->

# 二、通配符

| 通配符                         | 描述                   |
| ------------------------------ | ---------------------- |
| %                              | 替代 0 个或多个字符    |
| _                              | 替代一个字符           |
| [*charlist*]                   | 字符列中的任何单一字符 |
| [^*charlist*] 或 [!*charlist*] | 不在字符列             |

```mysql
-- LIKE'%inger' 将搜索以字母 inger 结尾的所有字符串
-- LIKE'_heryl' 将搜索以字母 heryl 结尾的所有六个字母的名称
-- LIKE'[CK]ars[eo]n' 将搜索下列字符串：Carsen、Karsen、Carson 和 Karson
-- LIKE'[M-Z]inger' 将搜索以字符串 inger 结尾、以从 M 到 Z 的任何单个字母开头的所有名称
-- LIKE'M[^c]%' 将搜索以字母 M 开头，并且第二个字母不是 c 的所有名称
```

<br/>

# 三、例子

```mysql
-- 搜索 empno 等于 7900 的数据
Select * from emp where empno=7900;

-- SMITH 用单引号引起来，表示是字符串，字符串要区分大小写
-- 使用 BINARY 关键字来设定WHERE子句的字符串比较是区分大小写的
Select * from emp where ename='SMITH';
SELECT * from emp WHERE BINARY ename='SMITH';

Select * from emp where sal > 2000 and sal < 3000;
Select * from emp where sal > 2000 or comm > 500;
select * from emp where not sal > 1500;

-- 查询 emp 表中 comm 列中的空值
Select * from emp where comm is null;

-- 大于等于 1500 且小于等于 3000
Select * from emp where sal between 1500 and 3000;

-- 查询 EMP 表 SAL 列中等于 5000，3000，1500 的值
Select * from emp where sal in (5000,3000,1500);

-- Like模糊查询
Select * from emp where ename like '_M%';
```

<br/>

# 四、不带比较运算符

WHERE子句并不一定带比较运算符，当不带运算符时，会执行一个隐式转换。**当0时转化为 false，当其他值是转化为true。** 

```mysql
-- 返回一个空集，因为每一行记录WHERE都返回false
SELECT studentNO FROM student WHERE 0;

-- 返回student表所有行记录的studentNO列。因为每一行记录WHERE都返回true。
SELECT  studentNO  FROM student WHERE 1;
SELECT studentNO FROM student WHERE 'abc';
```

