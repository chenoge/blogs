---
title: css-transform与坐标系统
date: 2018-03-21 11:55:31
tags: [css,transform,坐标系]
---

# 一、 坐标系统

我们很熟悉的网页是平面的，一个DOM元素会有一个初始坐标系（initial coordinate system）： 

![](css-transform与坐标系统\bVrVnk.png)

**每一个DOM元素都有一个这样的初始坐标系。其中，原点位于元素的左上角，z轴指向观察者。**初始坐标系的z轴并不算是三维空间，而是像z-index那样作为参照，决定网页元素的绘制顺序，绘制顺序靠后的元素将覆盖绘制顺序靠前的。 

<br/>

在使用transform的时候，情况则有所不同。transform所参照的并不是初始坐标系，而是一个新的坐标系： 

![](css-transform与坐标系统\bVrVnl.png)

transform所用的这个坐标系，相比初始坐标系，x、y、z轴的指向都不变，只是原点位置移动到了元素的正中心。**如果想要改变这个坐标系的原点位置，使用transform-origin**。**transform-origin的默认值是50% 50%**，因此，默认情况下，transform坐标系的原点位于元素中心。 

<br/>

# 二、变换函数顺序

我们都可能像

```css
 transform: rotateY(45deg)  translateX(100px); 
```

这样使用多个变换函数。这种时候，需要意识到变换函数的顺序。这是因为，**每一个变换函数不仅改变了元素，同时也会改变和元素关联的transform坐标系**，当变换函数依次执行时，<font color=#A52A2A size=4 >**后一个变换函数总是基于前一个变换后的新transform坐标系**</font>。 

<br/>

# 三、transform-origin

transform-origin属性是所有矩阵计算的一个重要依据点 

```css
.anim_image {
    -webkit-transition: all 1s ease-in-out;
    -moz-transition: all 1s ease-in-out;
    -o-transition: all 1s ease-in-out;
    transition: all 1s ease-in-out;
    cursor:pointer;
}

.anim_image_top {
    position: absolute;
    -webkit-transform: scale(0, 0);
    opacity: 0;
    filter: Alpha(opacity=0);
}

.anim_box:hover .anim_image_top , 
.anim_box_hover .anim_image_top {
    opacity: 1;
    filter: Alpha(opacity=100);
    -webkit-transform: scale(1, 1);
    -webkit-transform-origin: top right;        
}
	
	
.anim_box:hover .anim_image_bottom, 
.anim_box_hover .anim_image_bottom {
    -webkit-transform: scale(0, 0);
    -webkit-transform-origin: bottom left;
}
```

```html
<div id="testBox" class="demo anim_box">
    <img class="anim_image anim_image_top" src="/ps6.jpg" />
	<img class="anim_image anim_image_bottom" src="/ps4.jpg" />
</div>
```

Demo ：<http://www.zhangxinxu.com/study/201011/css3-transition-animate-demo-11.html> 

