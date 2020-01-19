---
title: shell-select
date: 2020-01-19 16:45:36
tags: [linux,shell,select]
---

####  select in 循环

select in 循环用来增强交互性，它可以显示出带编号的菜单，用户输入不同的编号就可以选择不同的菜单，并执行不同的功能。 

 Shell select in 循环的用法如下： 

```
select variable in value_list
do
    statements
done
```

 注意：select 是无限循环（死循环），输入空值，或者输入的值无效，都不会结束循环，只有遇到 break 语句，或者按下` Ctrl+D` 组合键才能结束循环。 

<!--more-->



```shell
#!/bin/bash
# Set PS3 prompt
PS3="Enter the space shuttle to get more information : "

# set shuttle list
select shuttle in columbia endeavour challenger discovery atlantis enterprise pathfinder
do
    echo "$shuttle selected"
    break
done
```

```shell
#!/bin/bash
PS3="What is your favourite OS?"
select name in "Linux" "Windows" "Mac OS" "UNIX" "Android"
do
    case $name in
        "Linux")
            echo "Linux是一个类UNIX操作系统，它开源免费，运行在各种服务器设备和嵌入式设备。"
            break
            ;;
        "Windows")
            echo "Windows是微软开发的个人电脑操作系统，它是闭源收费的。"
            break
            ;;
        "Mac OS")
            echo "Mac OS是苹果公司基于UNIX开发的一款图形界面操作系统，只能运行与苹果提供的硬件之上。"
            break
            ;;
        "UNIX")
            echo "UNIX是操作系统的开山鼻祖，现在已经逐渐退出历史舞台，只应用在特殊场合。"
            break
            ;;
        "Android")
            echo "Android是由Google开发的手机操作系统，目前已经占据了70%的市场份额。"
            break
            ;;
        *)
            echo "输入错误，请重新输入"
    esac
done
```

