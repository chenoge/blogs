---
title: CSS-blend-mode
date: 2018-05-15 15:27:27
tags: [css,background-blend-mode,mix-blend-mode]
---

# 一、mix-blend-mode
描述了元素的**内容**应该如何与<font color=#A52A2A size=4 >**元素的直接父元素</font>**和<font color=#A52A2A size=4 >**元素的背景**</font>混合。

``` css 
mix-blend-mode: normal;          //正常
mix-blend-mode: multiply;        //正片叠底
mix-blend-mode: screen;          //滤色
mix-blend-mode: overlay;         //叠加
mix-blend-mode: darken;          //变暗
mix-blend-mode: lighten;         //变亮
mix-blend-mode: color-dodge;     //颜色减淡
mix-blend-mode: color-burn;      //颜色加深
mix-blend-mode: hard-light;      //强光
mix-blend-mode: soft-light;      //柔光
mix-blend-mode: difference;      //差值
mix-blend-mode: exclusion;       //排除
mix-blend-mode: hue;             //色相
mix-blend-mode: saturation;      //饱和度
mix-blend-mode: color;           //颜色
mix-blend-mode: luminosity;      //亮度
mix-blend-mode: initial;         //初始
mix-blend-mode: inherit;         //继承
mix-blend-mode: unset;           //复原
```

<br/>

# 二、background-blend-mode 

定义该元素的<font color=#A52A2A size=4 >**背景图片</font>**，以及<font color=#A52A2A size=4 >**背景色**</font>如何混合，属性值和mix-blend-mode一样。



