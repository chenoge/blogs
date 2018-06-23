---
title: CSS-background-position
date: 2018-05-25 23:27:16
tags: [css,background-position]
---

# 一、先上图

![background-position](CSS-background-position\background-position.png)

图片上的标注信息已经告诉大家很详细的信息了。示例中容器具备下述特性： 

- div容器尺寸410px x 210px，边框宽度10px
- 容器背景图尺寸100px x 100px
- 第一张背景图background-position:10px 10px；第二张背景图background-position: center
- 其中黑白格子尺寸是10px x 10px

<br/>

<!--more--> 

# 二、特别注意

background-position取值除了长度值（<length>）和关键词之外，还可以取值为百分比值。

当背景图片尺寸（background-size）不做任何的重置（也就是100% 100%）时，**水平百分比的值等于<font color=#A52A2A size=4 >容器宽度百分比值减去背景图片宽度百分比值</font>。垂直百分比的值等于容器高度百分比值减去背景图片高度百分比值。**比如前面的示例，如果取值background-position: 75% 50%;，背景图片的起始位置： 

- **水平位置（x轴）：(410 - 100) * 75% = 232.5px**
- **垂直位置（y轴）：(210 - 100) * 50% = 55px**