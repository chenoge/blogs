---
title: git-tag命令
date: 2019-05-07 15:09:34
tags: [git,tag]
---

##### 创建标签

```cmd
# 默认标签是打在最新提交的commit上的
git tag <tagname>

# 在特定commit_id上打标签
git tag <tagname> commit_id

# 创建带有说明的标签
git tag -a <tagname> -m "msg" commit_id
```

<br/>



##### 查看标签

```cmd
# 查看全部标签
# 标签不是按时间顺序列出，而是按字母排序的
git tag

# 查看某个标签信息
git show <tagname>
```

<br/>



##### 删除标签

```cmd
git tag -d <tagname>
```

<!--more-->

<br/>



##### 远程推送标签

```cmd
# 推送某个标签到远程
git push origin <tagname>

# 一次性推送全部尚未推送到远程的本地标签
git push origin --tags

# 删除一个远程标签
git push origin :refs/tags/<tagname>
```

<br/>





