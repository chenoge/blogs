---
title: koa2-mysql连接池
date: 2018-04-12 15:09:51
tags: [koa2,mysql]
---

# 一、创建数据库会话

[mysql.js](https://github.com/mysqljs/mysql#install)

```javascript
const mysql = require('mysql')
const connection = mysql.createConnection({
  host     : '127.0.0.1',   // 数据库地址
  user     : 'root',    // 数据库用户
  password : '123456'   // 数据库密码
  database : 'my_database'  // 选中数据库
})

connection.connect();
	
// 执行sql脚本对数据库进行读写 
connection.query('SELECT * FROM my_table',  (error, results, fields) => {
    if (error) throw error
    // connected! 
    // 结束会话
    connection.release() 
});
```

<br/>

# 二、创建数据连接池

一般情况下操作数据库是很复杂的读写过程，不只是一个会话，如果直接用会话操作，就需要每次会话都要配置连接参数。所以这时候就需要连接池管理会话。

```javascript
const mysql = require('mysql')
// 创建数据池

const pool  = mysql.createPool({
  host     : '127.0.0.1',   // 数据库地址
  user     : 'root',    // 数据库用户
  password : '123456'   // 数据库密码
  database : 'my_database'  // 选中数据库
})

// 在数据池中进行会话操作
pool.getConnection(function(err, connection) {
    connection.query('SELECT * FROM my_table',  (error, results, fields) => {
        // 结束会话
        connection.release();
        // 如果有错误就抛出
        if (error) throw error;
    })
})
```

