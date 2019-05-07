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
# 查看标签,可加上参数 -l(列表形式列出） -n(附加说明)
# 标签不是按时间顺序列出，而是按字母排序的
git tag [-l -n]

# 查看某个标签信息
git show <tagname>

# 查看符合检索条件的标签 
git tag -l 1.*.* 
```

<br/>



##### 删除标签

```cmd
# 删除本地标签
git tag -d <tagname>

# 删除一个远程标签
git push origin :refs/tags/<tagname>

# Git v1.7.0 
# 删除一个远程标签
git push origin --delete tag <tagname>

# 删除一个远程分支
git push origin --delete <branchName>
```

<!--more-->

<br/>



##### 远程推送标签

```cmd
# 推送某个标签到远程
git push origin <tagname>

# 一次性推送全部尚未推送到远程的本地标签
git push origin --tags
```

<br/>





