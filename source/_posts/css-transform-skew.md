---
title: css-transform-skew
date: 2018-03-23 11:18:54
tags: [css,transform]
---

# 一、skew

**skew所用的坐标系，纵向是X轴，横向是Y轴**，与常见的坐标系是反着的。比如：

- skewX(30deg)      表示X轴朝**逆时针方向<font color=#A52A2A size=4 >旋转**</font>30deg，坐标系上的物体也会随着X轴旋转。
- skewY(10deg)      表示Y轴朝**顺时针方向<font color=#A52A2A size=4 >旋转**</font>10deg，坐标系上的物体也会随着Y轴旋转。

<br/>

<!--more--> 

#### skewX(30deg) 

![](css-transform-skew\8da73888f08c712cd2b86af3e02b5d0b_hd.jpg)

 

#### skewY(10deg)

![](css-transform-skew\36147026b20f167d83306bc7c251c6eb_hd.jpg)



#### 加在一起的效果是 

![](css-transform-skew\0bfc4a49f06867291c61e0e0e7c801f5_hd.jpg)

<br/>

# 二、对比

- **skewX(30deg)**：X轴逆时针方向<font color=#A52A2A size=4 >旋转</font>30deg（X轴旋转）
- **rotateX(*30deg*)** ：坐标系上的**点绕着X轴旋转**30deg（点旋转）
- **translateX(*30px*)** ：X轴沿着**轴方向**平移30px（X轴平移）
- **scaleX(*2*)** ：X轴放大2倍（X轴缩放）

**注：transform-origin（坐标系原点）不同，视觉效果往往大不一样**

