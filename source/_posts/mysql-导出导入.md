---
title: mysql-导出导入
date: 2019-06-11 12:12:29
tags: [mysql]
---

#### 导出数据

导出文件默认是存在`mysql\bin`目录下

##### 导出数据库

```
# mysqldump -u 用户名 -p 数据库名 > 导出的文件名
mysqldump -u user_name -p123456 database_name > outfile_name.sql
```



##### 导出表

```
mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
mysqldump -u user_name -p database_name table_name > outfile_name.sql
```



##### 导出数据库结构

```
mysqldump -u user_name -p -d –add-drop-table database_name > outfile_name.sql
# -d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table
```

<!--more-->

<br/>



#### 导入数据

#####  mysql 命令

```sql
# mysql -u用户名 -p密码 < 待导入文件.sql
mysql -uroot -p123456 < runoob.sql
```

<br/>



##### source 命令

```sql
mysql> create database abc;      # 创建数据库
mysql> use abc;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /home/abc/abc.sql  # 导入备份数据库
```

