---
title: shell-并行执行
date: 2020-01-17 12:26:30
tags: [shell,nohup]
---

#### 后台运行

 在shell脚本中当我们需要把一个任务放在后台运行时，通常我们会使用&符号 

```
subcommand &
```

 此时**主进程**会继续往下执行，而b会在后台启动运行 

 于此同时，我们常会看到`nohup`会和后台任务一起使用，格式是： 

```
nohup subcommand &
```



#### nohup

 nohup起两个作用： 

1. 正如名字所声称的，**忽略所有发送给子命令的挂断（SIGHUP）信号** 
2.  重定向子命令的标准输出(stdout)和标准错误(stderr) 。subcommand的标准输出和标准错误被重定向到`nohup.out`文件；如果没有使用nohup方式，则subcommand的标准输出和标准错误是复用父进程的标准输出和标准错误。 

<!--more-->



#### wait命令

 `wait`命令可以使当前`shell`进程挂起，等待所指定的由当前`shell`产生的子进程退出后，`wait`命令才返回。`wait`命令的参数可以是进程`ID`或是`job` 

```
wait [-n] [jobspec or pid …]
```



- wait命令的参数可以是**进程ID**或是**job specification**。举例如下：

  ```
  root# sleep 10 &
  [3] 876
  root# wait 876
  [3]+  Done                    sleep 10
  
  root# sleep 20 &
  [1] 877
  root# wait %1
  [1]+  Done                    sleep 20 
  ```

  

-  在`Bash shell`脚本中启动多个后台进程（使用`&`），然后调用`wait`命令，等待所有后台进程都运行完毕，`Bash shell`脚本再继续向下执行 

  ```
  command1 &
  command2 &
  wait
  ```



#### sleep命令

```
sleep [--help] [--version] number[smhd]
```

- --help : 显示辅助讯息
- --version : 显示版本编号
- number : 时间长度，后面可接 s、m、h 或 d
- 其中 s 为秒，m 为 分钟，h 为小时，d 为日数