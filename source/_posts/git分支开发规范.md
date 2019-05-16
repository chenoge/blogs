---
title: git分支开发规范
date: 2018-12-19 22:15:49
tags: [git]
---

#### 分支构成

`master`和`develop`分支一直存在，且名称不会变化，一般不直接修改这2个分支，由其他分支合并而来。`feature、release、hotfix`分别用于**功能点开发、优化**，**特定版本测试**，**线上问题紧急处理**。同一类型的分支会产生多个。

<!--more-->

<br/>



#### 分支划分

- master：与线上版本保持绝对一致
- develop：开发分支，由`release、feature、hotfix`分支合并过后的代码
- feature：实际功能点开发分支
  - 建议每个功能新建一个feature， **具有关联关系的功能公用一个feature分支**
- release：每一次开发完成之后，从develop创建出来的分支，以此分支为基准，进行测试
-  hotfix：该分支主要用于修复线上bug

<br/>



#### 分支与环境

- 生产环境：`master`分支

- 测试环境：`release`分支和`hotfix`分支

<br/>



#### 命名规范约定

- `feature`分支命名：`feature/name`
- `release`分支命名：`release/name`
- `hotfix`分支命名：`hotfix/name`

比如有一个「**优化分布式Session**」的需求，可在develop分支的基础上创建新分支 `feature/optimize_distributed_session`进行开发，开发完成后合并到`develop`分支。

<br/>



#### 处理流程

##### 1.master分支

主分支，与线上运行的版本始终保持一致，任何时候都不要直接修改master分支。

一个版本的release分支、hotfix分支开发完成后，会合并代码到master分支，也就是说**master分支主要来源于release分支和hotfix分支**。



##### 2.develop分支

开发分支，始终保持最新完成以及bug修复后的代码，新增功能时基于该分支创建feature分支。

一个版本的release分支、hotfix分支开发完成后，也会合并到develop分支，另外，一个版本的feature功能开发完成后，也会合并到develop分支。也就是说**develop分支来源于feature、release、hotfix分支**。



##### 3.feature分支

开发新功能或优化现有功能时，会创建feature分支，以develop为基础创建。一般会有多个功能同时开发，但上线时间可能不同，**在适当的时候将特定的feature分支合并到develop分支，并创建release分支，进入测试状态**。



##### 4.release分支

当一组feature开发完成，会首先合并到develop分支，开始**进入提测阶段时，会创建release分支**。

以release分支代码为基准提测，测试过程中若存在bug需要修复，则**直接由开发者在release分支修复并提交**。

测试完成之后，**合并release分支到master和develop分支**，此时master为最新代码，用作上线。



##### 5.hotfix分支

线上出现紧急问题时，需要及时修复，**以master分支为基线，创建hotfix分支**，修复完成后，需要合并到master分支和develop分支。

<br/>



#### 注意点

1. develop分支已存在未上线的feature代码，此时需要紧急上线一个新功能，但develop的代码不能上，如何处理 ？
   - 以master为基线创建feature， 在完成之后，代码合并到master分支
   - 为了保证develop是最新代码，需要从master合并到develop分支



2. 以develop为基线，创建了f1和f2两个feature分支之后，f1、f2开发一半的时候，发现两个分支代码需要有依赖怎么办 ？
   - 最好在开发开始前确定两个功能是否相关,若相关则只创建一个分支,两个功能在一起开发
   - 如果已经创建，则需要合并到一个分支
   - 一定要保证commit历史记录的整洁，代码合并时，根据情况选择merge或rebase
   - 使用rebase注意，一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行衍合操作

![](git分支开发规范\1.png)