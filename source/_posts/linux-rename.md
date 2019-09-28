---
title: linux-rename
date: 2019-09-28 22:29:17
tags: [linux,rename]
---

#### 语法

```shell
rename $1 $2 $3
```

- `$1`: 要被取代的关键字  
- `$2`: 新的关键字 
-  `$3`: 当名字符合这个规则的才取代  

```
# 把IMG001.jpg,IMG002.jpg… 换成img001.jpg,img002.jpg…   
rename IMG img IMG*

# 把档案foo1, ..., foo9, foo10, ..., foo278
# 改成foo001, ..., foo009, foo010, ..., foo278
rename foo foo0 foo?
rename foo foo0 foo??
```

```
# 将当前目录下.sh后缀的文件，变成.php  
rename 's/\.sh/\.php/' *

# 给www目录下的.php加上bak后缀  
rename 's/$/\.bak/' /home/www/*.php

# 给当前目录下的.bin后缀文件加上bak_前缀  
rename 's/^/bak_/' *.bin

# 批量删除当前目录下所有文件的.bin 后缀  
rename 's/\.bin$//' *

# 修改当前目录所有文件名为小写  
rename 's/A-Z/a-z/' *
```

