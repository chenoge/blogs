---
title: css-盒子垂直水平居中
date: 2018-03-5 22:19:57
tags: [css]
---

1、定位+盒子宽高已知 

```css
position: absolute; left: 50%; top: 50%; 
margin-left:-自身一半宽度; margin-top: -自身一半高度;
```

2、table-cell布局  

```css
/*父级*/
display: table-cell; vertical-align: middle; 
/*子级*/
margin: 0 auto;
```

3、定位 + transform ; 适用于 子盒子 宽高不定时 

```css
position: absolute;
 /*top和left偏移各为50%*/
top: 50%;
left: 50%; 
/*translate(-50%,-50%) 偏移自身的宽和高的-50%*/
transform: translate(-50%, -50%);
```

4、flex 布局 

```css
/*父级：*/
/*flex 布局*/
display: flex;
/*实现垂直居中*/
align-items: center;
/*实现水平居中*/
justify-content: center;
```

5、水平方向上居中  

```css
margin-left : 50% ;
transform: translateX(-50%);
```

