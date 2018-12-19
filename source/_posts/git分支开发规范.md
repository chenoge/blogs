---
title: git分支开发规范
date: 2018-12-19 22:15:49
tags: [git]
---

#### 分支管理

1、master分支

    部署生产环境的分支，这个分支只能从其他分支合并，如develop/release/hotfix，不能在这个分支直接修改

<br/> 

2、develop分支

    我们的主开发分支，是一个稳定的版本，通常由release分支合并过来，通常发到sit/uat环境进行测试，然后合并到master分支

<br/>

<!--more-->

3、hotfix分支

    主要是修复线上紧急bug的分支，此分支来自master分支，然后合并到master和develop

 <br/>

4、release分支

    主要是多人协作开发的大功能分支，此分支来自develop，合并到develop分支

 <br/>

5、feature分支

    主要是个人新功能开发的分支，如果多人开发，从release分支上拉，开发完成，合到release分支，如果单人开发，release和feature分支是相同的

<br/>

`master`、`develop` 分支大部分情况下都会保持一致，只有在**上线前的测试阶段** `develop` 比 `master` 的代码要多，一旦测试没问题，准备发布了，这时候会将 `develop` 合并到 `master `上。

但是我们发布之后又会进行下一版本的功能开发，开发中间可能又会遇到需要紧急修复 bug ，一个功能开发完成之后突然需求变动了等情况，所以除了以上` master` 和 `develop` 两个主要分支以外，还提出了以下三个辅助分支：`hotfix`、`release`、`feature`。

<br/>

#### 分支命名

除了主要分支的名字是固定的之外，派生分支是需要自己命名的，采用如下形式：

- **feature :** 按照功能点（而不是需求）命名` feature/` ；如 `feature/weixin_recharge`
- **hotfix :** 通过平台生成的**问题编号**来命名；如 `hotfix/#1`

