---
title: CSS-linear-gradient
date: 2018-05-15 13:53:42
tags: [css,linear-gradient]
---

# 一、语法

``` css
background-image: linear-gradient([<angle>|<side-or-corner>,]?<color-stop>[,<color-stop>]+);
```

- []在正则中表示一个字符类，这里，你可以理解为一个小单元。 

- |表示候选。也就是“或者”的意思，要么前面的，要么就后面的。 

- ?为量词，表示0个或1个。 

- +也是量词，表示1个或者更多个。因此，终止颜色是不可缺少的。 

- <>中的是关键字，主要是让开发人员知道这里应该放些什么内容。

<br/>

<!--more--> 

# 二、angle

![gradient-1](CSS-linear-gradient\gradient-1.png)





**C点渐变容器中心点，A是过C点垂直线与<font color=#A52A2A size=4 >过C点渐变线</font>的夹角，这个角称为渐变角度。** 





![](CSS-linear-gradient\7B0CC41A-86DC-4E1B-8A69-A410E6764B91.jpg)

<br/>

# 三、side-or-corner

side-or-corner中文意思就是“边或角”，可选值有： [left | right] || [top | bottom]，有如下的写法或组合：

left, right, top, bottom, left top, left bottom, right top, right bottom. 分别表示，从左往右，从右往左，从上往下，从下往上，从左上往右下，从……

<br/>

# 四、color-stop

**渐变关键颜色结点，语法为：<color> [ <percentage> | <length> ]**

<br/>

# 五、角度坐标与位置关系

对于<font color=#A52A2A size=4 >斜向线性渐变</font>，点到点的渐变不是直接把点的横坐标放上去就可以的。因为当<font color=#A52A2A size=4 >渐变倾斜的时候，渐变的起止点的坐标也发生变化了</font>。下图是官方规范的一张示意图，演示的是45deg渐变的起止点以及方向。 

![gradient-diagram](CSS-linear-gradient\gradient-diagram.png)



记住一个关键点，**渐变的起点和终点（默认）在过中心的<font color=#A52A2A size=4 >渐变线的垂直线</font>上**，于是，我们就可以确定起点与终点的位置了。按照这个理解，我们就可以画出400*300 div上135deg起始点在哪里，然后再确定(100,100)和(200,200)的位置就轻松多了。如下示意图： 



![](CSS-linear-gradient\100-300-what-mean.png)



一图顶前言，反正上面这张图我是看懂了。于是，我们的坐标起止点值其实就变成了，黑色括弧的长度以及紫色括弧的长度值分别多少！虽然很多人不喜欢数学，但是几何应该都还不错，我们来一起算一下……



结果为，起点：

``` javascript
100 * Math.sqrt(2) = 141.4213562373095;
```



终点为：

``` javascript
200 * Math.sqrt(2) = 282.842712474619;
```



CSS用上：

``` css
{
    background-image: linear-gradient(135deg, red 141.42px, yellow 282.84px);
}
```

