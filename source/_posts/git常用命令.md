---
title: git常用命令
date: 2019-02-28 12:01:18
tags: [git]
---

#### 1、查看历史

- `git log`：查看**提交历史**，以便确定要回退到哪个版本
- `git reflog`：查看**命令历史**，以便确定要回到未来的哪个版本

<br/>



#### 2、分支操作

- 查看分支：`git branch` 
- 创建分支：`git branch <name>` 
- 切换分支：`git checkout <name> ` 
- 创建+切换分支：`git checkout -b <name> ` 
- 创建+切换+关联远程分支：`git checkout -b 本地分支名 origin/远程分支名`
- 合并某分支到当前分支：`git merge <name> ` 
- 删除分支：`git branch -d <name> ` 

<br/>



#### 3、关联远程仓库

- 查看关联的远程仓库的名称：`git remote` 
- 查看关联的远程仓库的详细信息 ：`git remote -v ` 
- 添加远程仓库的关联：`git remote add origin <url>` 
- 删除远程仓库的关联：`git remote remove <name>`  
- 更新远程仓库的`url` ：`git remote set-url origin <newurl> ` 

<!--more-->

<br/>



#### 4、推送更新

`git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相似。

```
git push <远程主机名> <本地分支名>:<远程分支名>
```


##### 更新远程分支

```
git push origin master : refs/for/master
# 即是将本地的master分支推送到远程主机origin上的对应master分支
#origin 是远程主机名，第一个master是本地分支名，第二个master是远程分支名
```


##### 新建远程分支

```
git push origin master
# 如果远程分支被省略，表示将本地分支推送到与之存在追踪关系的远程分支
# 如果该远程分支不存在，则会被新建
```


##### 删除远程分支

```
git push origin ：refs/for/master
# 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支
# 等同于 git push origin --delete master
```



##### 默认当前分支

```
git push origin
# 如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略
# 将当前分支推送到origin主机的对应分支 
```



##### 默认主机名

```
git push
# 如果当前分支只有一个远程分支，那么主机名都可以省略
# 以使用git branch -r ，查看远程的分支名
```



##### 默认主机

```
git push -u origin master
# 如果当前分支与多个主机存在追踪关系，则可以使用 -u 参数指定一个默认主机
# 这样后面就可以不加任何参数使用git push
```



##### 强行推送

```
git push --force origin
# 如果远程主机的版本比本地版本新，推送时Git会报错
# 如果你一定要推送，可以使用-f | --force选项
```



##### 推送所有分支

```
git push --all origin
# 当遇到这种情况就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要 -all 选项
```

<br/>



#### 5、回退(reset )与反做(revert )

##### 回退

- 回退本地分支：`git reset --hard commit_id` 
- 强制推送到远程分支：`git push -f` 

**适用场景：** 如果想恢复到之前某个提交的版本，且那个版本之后提交的版本都不要了

 

##### 反做

- 撤销某次提交：`git revert commit_id`  
- 撤销最近一次提交 ：`git revert HEAD ` 
- 撤销最近的N个提交：`git revert HEAD~3` 

通过创建一个新的版本，这个版本的内容与我们要回退到的目标版本一样，但是HEAD指针是指向这个新生成的版本，而不是目标版本 。 

**适用场景：** 如果我们想恢复之前的某一版本（该版本不是merge类型），但是又想保留该目标版本后面的版本，记录下这整个版本变动流程，就可以用这种方法。 

<br/>



#### 6、忽略已提交文件

```cmd
# git rm -r --cached 要忽略的文件
git rm -r --cached . # 删除追踪状态
git add . 
git commit -m "fixed untracked files"
```