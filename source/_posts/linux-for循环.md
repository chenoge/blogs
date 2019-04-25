---
title: linux-for循环
date: 2019-04-25 11:46:51
tags: [linux,for]
---

#### for循环结构

```shell
# 语法一
# for 变量 in 值1 值2 值3..
#    do
#      程序块儿
#    done

#!/bin/bash
for i in 1 2 3 4 5 
  do
    echo "$i-->$(uptime)"
  done
```
<!--more-->

<br/>



```shell
# 语法二
# for 变量 `命令`
#    do
#      程序块儿
#    done

#!/bin/bash
for i in `ls ./*.tar.gz` 
   do
     tar -zxvf $i >/dev/null
   done

for name in `ls | grep "name"`; do mv $name ./temp/; done
```

<br/>



```shell
# 语法三
# for ((初始值; 循环控制; 变量变化))
#   do
#     程序块儿
#   done

#!/bin/bash
# 注意变量赋值的时候,=两边绝对不能有空格
sum=0
for (( i=1; i<=100; i++ ))
  do  
   sum=$(( $sum + $i ))
  done
echo "1+2+3+...+100=$sum"
```

注：linux下for循环中可以使用`break`和 `continue`关键字来跳出循环， 和java 用法一致
