---
title: linux用户账号相关操作
date: 2019-03-06 17:28:32
tags: [linux,useradd,usermod,passwd,id]
---

#### `useradd` | `adduser`命令 

`useradd` | `adduser`命令用来建立用户帐号和创建用户的起始目录，**使用权限是超级用户**。 

```
useradd  [-d home] [-s shell] [-c comment] [-m [-k template]] 
         [-f inactive] [-e expire ] [-p passwd] [-r] name
```

`-c`：加上备注文字，备注文字保存在`passwd`的备注栏中

`-d`：指定用户登入时的主目录，替换系统默认值`/home/<用户名>`

`-D`：变更预设值

`-e`：指定账号的失效日期，日期格式为`MM/DD/YY`，例如`06/30/12`。缺省表示永久有效

`-f`：指定在密码过期后多少天即关闭该账号。

- 默认值为`-1`

- 如果为0账号立即被停用
- 如果为`-1`则账号一直可用

`-g`：指定用户所属的群组。值可以使组名也可以是`GID` 

- 用户组必须已经存在的，期默认值为`100`，即`users`

`-G`：指定用户所属的**附加群组** 

`-m`：自动建立用户的**登入目录** 

`-M`：不要自动建立用户的登入目录

`-n`：取消建立以用户名称为名的群组

`-r`：**建立系统账号** 

`-s`：指定用户登入后所使用的`shell`。默认值为`/bin/bash` 

`-u`：指定用户ID号。该值在系统中必须是唯一的

- 0~499默认是保留给系统用户账号使用的，所以该值必须大于499

注：账号建好之后，再用`passwd`命令设定账号的密码

<!--more-->

<br/>

#### passwd命令

```
passwd [-k] [-l] [-u [-f]] [-d] [-S] [username]
```

- `-d` 删除密码
- `-f` 强制执行
- `-k` 更新只能发送在过期之后
- `-l` 停止账号使用
- `-S` 显示密码信息
- `-u` 启用已被停止的账户
- `-x` 设置密码的有效期
- `-g` 修改群组密码
- `-i` 过期后停止用户账号

<br/>

#### id命令

```
id [-gGnru][--help][--version][用户名称]
```

- `-g`或`--group` 　显示用户所属群组的ID
- `-G`或`--groups` 　显示用户所属附加群组的ID
- `-n`或`--name` 　显示用户，所属群组或附加群组的名称
- `-r`或`--real` 　显示实际ID
- `-u`或-`-user` 　显示用户ID
- `-help` 　显示帮助
- `-version` 　显示版本信息

<br/>

#### usermod 命令 

```
usermod [options] user_name
```

```
- a | -- append  把用户追加到某些组中，仅与-G选项一起使用

- c | -- comment  修改用户帐号的备注文字

- d | -- home  修改用户的家目录通常和-m选项一起使用

- e | -- expiredate  指定用户帐号禁用的日期，格式YY-MM-DD

- f | -- inactive  用户密码过期多少天后采用就禁用该帐号

- g | -- gid  修改用户的gid，改组一定存在

- G | -- groups  把用户追加到某些组中，仅与-a选项一起使用

- l | -- login  修改用户的登录名称

- L | -- lock  锁定用户的密码

- m | -- move - home  修改用户的家目录通常和-d选项一起使用

- s | -- shell  修改用户的shell

- u | -- uid  修改用户的uid，该uid必须唯一

- U | -- unlock  解锁用户的密码
```

```
 usermod -a -G www hexu   # 将hexu添加到www用户组
```

<br/>