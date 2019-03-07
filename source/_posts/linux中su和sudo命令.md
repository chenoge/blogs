---
title: linux中su和sudo命令
date: 2019-03-07 10:49:57
tags: [linux,su,sudo]
---

#### `su`命令

`Linux` `su`命令用于变更为其他使用者的身份，除 `root` 外，需要键入该使用者的密码。 

```
su [-fmp] [-c command] [-s shell] [--help] [--version] [-] [USER [ARG]]
```

- `-f `或` --fast` 不必读启动档（如 csh.cshrc 等），仅用于 csh 或 tcsh
- `-m -p` 或 `--preserve-environment` 执行 su 时不改变环境变数
- `-c command` 或` --command=command` 
  - 变更帐号为 `USER` 的使用者，并执行指令（`command`）后再变回原来使用者
- `-s shell` 或 `--shell=shell `
  - 指定要执行的 shell （bash csh tcsh 等）
  - 预设值为` /etc/passwd` 内的该使用者（USER） `shell` 
- `--help` 显示说明文件
- `--version` 显示版本资讯
- `-` 、`-l `或` --login` 
  - 这个参数加了之后，就好像是重新 login 为该使用者一样
  - 大部份环境变数都是以该使用者（USER）为主，并且**工作目录也会改变** 
  - 如果没有指定 `USER` ，内定是 `root` 
- `USER` 欲变更的使用者帐号
- `ARG` 传入新的 `shell` 参数

```shell
su -c ls root # 变更帐号为 root 并在执行 ls 指令后退出变回原使用者
su root -f # 变更帐号为 root 并传入 -f 参数给新执行的 shell
su - nginx # 变更帐号为 nginx 并改变工作目录至 nginx 的家目录（home dir）
```

<!--more-->

<br/>

#### `su `和` su - ` 区别

`su username` 

- 切换到指定用户，但是**当前目录**不会变化
- **环境变量**还是上一个用户的环境变量

`su - username` 

- 切换到指定用户，**当前目录**即刻切换成指定用户的家目录
- **环境变量**即刻切换到指定目录的环境变量

<br/>

#### `sudo`命令

```shell
sudo command
```

通过`sudo`命令，我们能把某些超级权限有针对性的下放，并且**不需要普通用户知道`root密码`，而是验证当前用户的密码** 。`sudo`是需要授权许可的，所以也被称为**授权许可的`su`**。

`sudo`执行命令的流程是当前用户切换到`root`，然后以`root`身份执行命令，执行完成后，直接退回到当前用户，而这些的**前提**是要通过`sudo`的配置文件`/etc/sudoers来进行授权。

<br/>

