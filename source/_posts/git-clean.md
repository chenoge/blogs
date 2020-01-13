---
title: git-clean
date: 2020-01-13 17:53:50
tags: [git clean]
---

#### git删除未跟踪文件

```cmd
# 删除 untracked files （-f：文件）
git clean -f
 
# 连 untracked 的目录也一起删掉（-d：目录）
git clean -fd

# 连 .gitignore 的untrack 文件/目录也一起删掉（-x：.gitignore中的文件/目录）
git clean -xfd

# 在用上述 git clean 前，建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
```

<!--more-->



#### 工作区、版本库

- 工作区（`Working Directory`）：就是电脑里的目录，一个文件夹就是一个工作区
- 版本库（`Repository`）：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
  Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。



#### 管理修改

- 用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区，可反复多次使用
- 用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

注：Git跟踪并管理的是修改，而非文件
