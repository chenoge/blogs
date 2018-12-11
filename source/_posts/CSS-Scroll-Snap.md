---
title: CSS-Scroll-Snap
date: 2018-12-11 20:34:55
tags: [CSS,scroll-snap]
---

`CSS Scroll Snap`是`CSS`中一个独立的模块，可以让**网页容器滚动停止的时候，自动平滑定位到指定元素的指定位置**，包含`scroll-*`以及`scroll-snap-*`等诸多CSS属性。 

`Scroll Snap`模块相关`CSS`属性可以分为两类，一类作用在滚动容器上，一拨作用在你希望有滚动定位点的子元素上。具体参见下表：

| 作用在滚动容器上                | 作用在定位子项上              |
| ------------------------------- | ----------------------------- |
| scroll-snap-type                | scroll-snap-align             |
| scroll-snap-stop                | scroll-margin/scroll-margin-* |
| scroll-padding/scroll-padding-* |                               |

<br/>

<!--more-->

#### 1. scroll-snap-type

`scroll-snap-type`作用就是确定是水平滚动定位，还是垂直滚动定位。支持的属性值如下：

- **none** ：默认值，表示滚动时候忽略捕捉点。 
- **x** ：捕捉水平定位点。 
- **y**  ：捕捉垂直平定位点。 
- **block** ： 捕捉和块状元素排列一个滚动方向的定位点，默认文档流下指的就是垂直轴。 
- **inline** ：捕捉和内联元素排列一个滚动方向的定位点，默认文档流下指的就是水平轴。 
- **both** ：横轴纵轴都捕捉。  
- **mandatory** ：强制定位，可选参数。无论是添加删除元素，或者滚动窗口较小，不足以放下子元素。  
- **proximity** ：表示“大约”，可选参数。 

<br/>

#### 2. scroll-snap-stop

`scroll-snap-stop`表示是否允许滚动容器忽略捕获位置。其中，支持的属性值：

- **normal** ：默认值。可以忽略捕获位置。  
- **always**：不能忽略捕获位置。且必须定位到第一个捕获元素的位置。  

<br/>

#### 3. scroll-snap-align

`scroll-snap-align`是作用在滚动容器子元素上的，表示捕获点是上边缘，下边缘，还是中间位置。其中，支持的属性值：

- **none**：默认值。不定义位置。  

- **start**：起始位置对齐，例如，垂直滚动，子元素和容器同上边缘对齐。  
- **end**：结束位置对齐，例如，垂直滚动，子元素和容器同下边缘对齐。  
- **center**：居中对齐。子元素中心和滚动容器中心一致。  

注：`scroll-snap-align`还支持同时使用两个属性值，例如：

```css
scroll-snap-align: start end;
```

此时，第一个属性值表示`block元素`排列方向（通常垂直），第二个属性值表示inline元素`的排列方向（通常水平）。

<br/>



 