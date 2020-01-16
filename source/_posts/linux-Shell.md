---
title: linux-Shell
date: 2020-01-07 20:45:30
tags: [Shell]
---

#### 一、Shell 变量

##### 定义变量

```shell
name="czh"
```

-  **定义变量**时，变量名不加美元符号 
- **二次赋值**时，变量名不加美元符号 
-  变量名和等号之间不能有空格 

##### 使用变量

```shell
name="czh" 
echo $name
echo "My name is ${name}"

readonly name # 使用 readonly 命令可以将变量定义为只读变量
unset name # 使用 unset 命令可以删除变量
```

- 使用一个定义过的变量，只要在变量名前面加美元符号即可 
-  变量名外面的花括号是可选的 

<!--more-->



##### Shell 字符串

```shell
string="abcd"
echo ${#string} # 获取字符串长度，输出 4
echo ${string:1:3} # 提取子字符串，输出 bcd
```



##### Shell 数组

```shell
array_name=(A B "C" D)
name0=${array_name[0]} # 读取数组元素值
length=${#array_name[*]} # 获取数组长度
```

- bash只支持一维数组，并且没有限定数组的大小

- 在 Shell 中，用括号来表示数组，数组元素用"**空格**"符号分割开 



#### 二、Shell 基本运算符

```shell
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```

-  原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 `awk` 和 `expr`
-  表达式和运算符之间要有空格 
- 乘号(*)前边必须加反斜杠(`\`)才能实现乘法运算； 

```shell
#!/bin/bash

a=10
b=20

if [ $a == $b ]
then
   echo "a 等于 b"
fi

if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```



##### 关系运算符

 假定变量 a 为 10，变量 b 为 20

| `-eq` | 检测两个数是否相等，相等返回 true                   | `[ $a -eq $b ]` 返回 false |
| ----- | --------------------------------------------------- | -------------------------- |
| `-ne` | 检测两个数是否不相等，不相等返回 true               | `[ $a -ne $b ]` 返回 true  |
| `-gt` | 检测左边的数是否大于右边的，如果是，则返回 true     | `[ $a -gt $b ]` 返回 false |
| `-lt` | 检测左边的数是否小于右边的，如果是，则返回 true     | `[ $a -lt $b ] `返回 true  |
| `-ge` | 检测左边的数是否大于等于右边的，如果是，则返回 true | `[ $a -ge $b ]` 返回 false |
| `-le` | 检测左边的数是否小于等于右边的，如果是，则返回 true | `[ $a -le $b ]` 返回 true  |



##### 布尔运算符

 假定变量 a 为 10，变量 b 为 20 

| `!`  | 非运算，表达式为 true 则返回 false，否则返回 true | `[ ! false ]` 返回 true                  |
| ---- | ------------------------------------------------- | ---------------------------------------- |
| `-o` | 或运算，有一个表达式为 true 则返回 true           | `[ $a -lt 20 -o $b -gt 100 ] `返回 true  |
| `-a` | 与运算，两个表达式都为 true 才返回 true           | `[ $a -lt 20 -a $b -gt 100 ] `返回 false |



##### 逻辑运算符

 假定变量 a 为 10，变量 b 为 20 

| &&   | 逻辑的 AND | `[[ $a -lt 100 && $b -gt 100 ]]` 返回 false |
| ---- | ---------- | ------------------------------------------- |
| \|\| | 逻辑的 OR  | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true  |



#####  判断上一个命令是否执行成功 

 shell中使用符号`$?`来显示上一条命令执行的返回值，如果为0则代表执行成功，其他表示失败。 

```shell
if [ $? -eq 0 ]; then
    echo "succeed"
else
    echo "failed"
fi
```



```shell
# 在 bash shell 中，$( ) 与` ` (反引号) 都是用来做命令替换用(commandsubstitution)的

# ${ }用于变量替换。一般情况下，$var 与${var} 并没有啥不一样

# $[] $(()) 都是进行数学运算的
```



#### 三、Shell输入

##### 基本读取

```shell
read -p "Enter your name:" name
```

- ` -p`指定提示语句
- `-n`限定字符个数
- `-t`设置等待时间
- `-s`不显示

注： 如果不指定**变量名**，那么read命令把接收到的输入放在环境变量`REPLY`中，我们可以正常使用环境变量`REPLY`。 



#####  输入多个数据 

```shell
read -p "Enter your name age id_card address:" name age id_card address 
```

- 如果输入数据个数过多，远大于变量个数，则多余的所有数据都给最后一个变量 



#####  读文件 

```shell
#!/bin/bash

count=1

cat test | while read line # cat 命令的输出作为read命令的输入, read读到的值放在line中

do

   echo "Line $count:$line"
   
   count=$[ $count + 1 ] # 注意中括号中的空格
   
done

echo "finish"

exit 0
```

- 每次调用read命令都会读取文件中的"一行"文本
- 当文件没有可读的行时，read命令将以非零状态退出



#### 四、shell函数

```
[ function ] funname [()]

{

    action;

    [return int;]

}
```

-  可以带`function fun() `定义，也可以直接`fun() `定义，不带任何参数
-  参数返回，可以显示加：return 返回；如果不加，将以最后一条命令运行结果，作为返回值 
-  return后跟数值`n（0-255 ）`



##### 舆情项目自动部署脚本

```shell
#!/bin/sh

# 配置免密
function setPasswordFree() {
  echo "Set up password-free login"
  echo "https://username:password@github.com" >~/.git-credentials
  git config --global credenttial.helper store
}

# 去掉免密
function removePasswordFree() {
  echo "Remove password-free login"
  rm -rf ~/.git-credentials
  git config --global credenttial.helper store
}

# 更新代码
function pull() {
  echo "Try to pull the code"
  git reset --hard
  git pull
}

# 跑程序
function run() {
  # 设置默认参数
  ENV=$1
  if [ $# -eq 0 ]; then
    ENV="prod"
  fi

  echo "The current environment parameter variable is ${ENV}"
  echo "Building program"
  mvn clean package -P $ENV
  cd target
  sh stop.sh
  sh start.sh
  tail -f EpsmWebApplication.log
}

setPasswordFree
pull
pullRet=$?
removePasswordFree

# 如果免密失效，转成手动输入
if [ $pullRet -eq 0 ]; then
  run
else
  echo "Password-free login is invalid, please enter it manually"
  pull
  run
fi
```
