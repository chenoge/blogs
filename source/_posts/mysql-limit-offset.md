---
title: mysql-limit-offset
date: 2018-04-14 17:09:51
tags: [mysql,limit,offset]
---

```mysql
select _column,_column from _table [where Clause] [limit N][offset M]
```

- `limit N` : 返回 N 条记录
- `offset M` : 跳过 M 条记录, 默认 M=0
- `limit N offset M` :从第 M条记录开始, 返回N 条记录
- `limit M,N ` : 相当于 **`limit N offset M`** , 从第 M 条记录开始, 返回 N 条记录

