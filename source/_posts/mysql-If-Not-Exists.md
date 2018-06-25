---
title: mysql-If-Not-Exists
date: 2018-04-14 13:07:43
tags: [mysql,If-Not-Exists,If-Exists]
---

# 一、创建数据库

```mysql
CREATE DATABASE IF NOT EXISTS RUNOOB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

**数据库的校验规则**

- `utf8_bin`：将字符串中的每一个字符用二进制数据存储，大小写**敏感**
- `utf8_genera_ci`：大小写**不敏感**（`ci:case insensitive`）
- `utf8_general_cs`：大小写**敏感**（`cs:case sensitive`）

<br/>

# 二、创建数据表

```mysql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
    `runoob_id` INT UNSIGNED AUTO_INCREMENT,
    `runoob_title` VARCHAR(100) NOT NULL,
    `runoob_author` VARCHAR(40) NOT NULL,
    `submission_date` DATE,
    PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

- `ENGINE=InnoDB`：使用`innodb`引擎

<br/>

# 三、判断数据库存在, 则删除

```mysql
drop database if exists db_name;
```

<br/>

# 四、判断数据表存在, 则删除

```mysql
drop table if exists table_name;
```

