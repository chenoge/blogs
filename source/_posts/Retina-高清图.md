---
title: Retina-高清图
date: 2017-12-09 21:32:24
tags: [retina,视网膜图]
---

# 一、高清图问题

1、理论上，**1个<font color=#A52A2A size=4 >位图像素</font>对应于1个<font color=#A52A2A size=4 >物理像素</font>，图片才能得到完美清晰的展示**。在普通屏幕下是没有问题的，但是在<font color=#A52A2A size=4 >**retina屏幕**</font>下就会出现位图像素点不够，从而导致图片模糊的情况。用一张图来表示： 

![](Retina-高清图\Fuex59zSiV9pbaJG-s9wg_UpCERP.jpg)

如上图：对于dpr=2的retina屏幕而言，<font color=#A52A2A size=4 >**1个位图像素对应于4个物理像素，由于单个位图像素不可以再进一步分割，所以只能就近取色，从而导致图片模糊</font>**(注意上述的几个颜色值)。 

<br/>

2、如果用了两倍图片，会怎样呢？很明显，在普通屏幕下，200×300(css pixel)img标签，所对应的物理像素个数就是200×300个，而两倍图片的位图像素个数则是200×300*4，所以就出现**一个物理像素点对应4个位图像素点**，所以它的取色也只能通过一定的算法(显示结果就是一张只有原图像素总数四分之一，我们称这个过程叫做**downsampling**)，肉眼看上去虽然图片不会模糊，但是会觉得图片缺少一些**锐利度**，或者是有点色差(但还是可以接受的)。



 ![Retina-高清图](Retina-高清图\FsYhT3m0Zq3ce-HLBOOlQfY9W2DD.jpg)

<br/>

# 二、 1px border问题

一张图来解释： 

![](Retina-高清图\FkiktwhAWrkJoZmYuiYG-DzWDfME.jpg)

上图中，**对于一条1px宽的直线，它们在屏幕上的<font color=#A52A2A size=4 >物理尺寸</font>(灰色区域)的确是相同的**，不同的其实是屏幕上最小的物理显示单元，即物理像素，所以对于一条直线，iphone5它能显示的最小宽度其实是图中的红线圈出来的灰色区域，用css来表示，理论上说是0.5px。设计师想要的retina下border: 1px;，其实就是1物理像素宽，对于css而言，可以认为是border: 0.5px;，这是retina下(dpr=2)下能显示的最小单位。

<br/>

**注：在不同的屏幕上(普通屏幕 vs retina屏幕)，css像素所呈现的大小(物理尺寸)是一致的，不同的是1个css像素所对应的物理像素个数是不一致的。** 

<br/>

# 三、字体问题

对于字体缩放问题，设计师原本的要求是这样的：**任何手机屏幕上字体大小都要统一**，所以我们针对不同的分辨率(dpr不同)，会做如下处理： 

```html
<meta name="viewport" content="width=device-width*dpr,initial-scale=1/dpr,maximum-scale=1/dpr, minimum-scale=1/dpr,user-scalable=no">
```

```css
font-size: 16px;
[data-dpr="2"] input {
    // font-size: calc(attr(data-dpr)*16px); 不支持
    font-size: 32px;
}
```

**注意，字体不可以用rem，误差太大了，且不能满足任何屏幕下字体大小相同**。为了方便，我们也会用less写一个mixin：



```css
.px2px(@name, @px) {
    @{name}: round(@px / 2) * 1px;
    
    [data-dpr="2"] & {
        @{name}: @px * 1px;
    } 
    
    // for mx3
    [data-dpr="2.5"] & {
        @{name}: round(@px * 2.5 / 2) * 1px;
    } 
    
    // for 小米note
    [data-dpr="2.75"] & {
        @{name}: round(@px * 2.75 / 2) * 1px;
    }
    
    [data-dpr="3"] & {
        @{name}: round(@px / 2 * 3) * 1px
    } 
    
    // for 三星note4
    [data-dpr="4"] & {
        @{name}: @px * 2px;
    }
}
```

