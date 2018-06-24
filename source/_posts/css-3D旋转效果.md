---
title: css-3D旋转效果
date: 2017-11-26 18:25:56
tags: [css,3D,变换,旋转,transform,perspective]
---

# 一、XYZ轴

以屏幕为平面，X轴的方向为宽的方向，Y轴的方向为高的方向，Z轴的方向垂直于屏幕

![](css-3D旋转效果\3d_axes.png)

<br/>

<!--more-->

# 二、css变换函数

- rotateX(angle)：绕着X轴旋转 

- rotateY(angle)：绕着Y轴旋转 

- rotateZ(angle)：绕着Z轴旋转 

- translateX(n)：沿着X轴位移 

- translateY(n)：沿着Y轴位移 

- translateZ(n)：沿着Z轴位移

<br/>

# 三、属性

#### 1、transform-style属性 

```css
transform-style: flat | preserve-3d
/*flat值为默认值，表示所有子元素在2D平面呈现。*/
/*preserve-3d表示所有子元素在3D空间中呈现。*/
```

- 如果对一个元素设置了`transform-style`的值为`flat`，则**该元素的所有子元素都将被平展到该元素的2D平面中进行呈现**。沿着X轴或Y轴方向旋转该元素将导致位于正或负Z轴位置的子元素显示在该元素的平面上，而不是它的前面或者后面。 
- 如果你的元素设置了`transform-style`值为`preserve-3d`，就**不能为了防止子元素溢出容器而设置overflow值为hidden**。

<br/>

#### 2、perspective属性 

```css
perspective：none | <length>
```

简单的理解为视距，**用来设置用户和元素3D空间Z平面之间的距离**。

**perspective属性的默认值为none，表示无限的角度来看3D物体**，但看上去是平的。

另一个值`<length>`接受一个长度单位大于0的值。而且其单位不能为百分比值。

<br/>

- `<length>`值越大，角度出现的越远，从而创建一个相当低的强度和非常小的3D空间变化。
- 反之，此值越小，角度出现的越近，从而创建一个高强度的角度和一个大型3D变化。 

<br/>

为了使用原始圆（轮廓）看起来出现在Z轴上（虚线圆），画布上的实体圆将扩大两部，如浅蓝色的圆。在底部图中显示，圆按比例缩小，致使虚线圆出现在画布后面，并且z的大小是距原始位置三分之一。 

**`perspective`用在舞台元素上（变形元素们的共同父元素）；`perspective()`就是用在当前变形元素上，并且可以与其他的transform函数一起使用。**

![](css-3D旋转效果\transform-21.jpg)

<br/>

#### 3、perspective-origin属性 

主要用来决定`perspective`属性的**源点角度**。它实际上设置了X轴和Y轴位置，在该位置观看者好像在观看该元素的子元素。 

![](css-3D旋转效果\transform-24.jpg)

<br/>

#### 4、backface-visibility属性 

```css
backface-visibility: visible | hidden
/**决定元素旋转背面是否可见 */
```

![](css-3D旋转效果\transform-28.jpg)

