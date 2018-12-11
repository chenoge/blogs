---
title: 自定义scrollbar
date: 2018-05-29 18:51:55
tags: [css,scrillbar]
---

#### 一、滚动条相关伪元素

- **::-webkit-scrollbar** —      整个滚动条.
- **::-webkit-scrollbar-button** —      滚动条上的按钮 (上下箭头).
- **::-webkit-scrollbar-thumb** —      滚动条上的滚动滑块.
- **::-webkit-scrollbar-track** —      滚动条轨道.
- **::-webkit-scrollbar-track-piece** —      滚动条没有滑块的轨道部分.
- **::-webkit-scrollbar-corner** —      当同时有垂直滚动条和水平滚动条时交汇的部分.
- **::-webkit-resizer** —      某些元素的corner部分的部分样式(例:textarea的可拖动按钮).

<br/>

<!--more-->

![自定义webkit滑动条](CSS-自定义webkit滑动条/scrollbar.png)

<br/>

``` css
/*定义滚动条高宽及背景高宽，分别对应横、竖滚动条的尺寸*/
::-webkit-scrollbar {
    width: 16px;
    height: 16px;
    background-color: #F5F5F5;
}

/*定义滚动条轨道内阴影+圆角*/
::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    background-color: #F5F5F5;
}

/*定义滑块内阴影+圆角*/
::-webkit-scrollbar-thumb {
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
    background-color: #555;
}
```

```css
/*类与伪元素搭配*/
.sidebar {
    position: fixed;
    border-right: 1px solid rgba(0,0,0,.07);
    overflow-y: auto;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    transition: transform .25s ease-out;
    width: 300px;
    z-index: 3;
}

.sidebar::-webkit-scrollbar {
    width: 4px
}

.sidebar::-webkit-scrollbar-thumb {
    background: transparent;
    border-radius: 4px
}

.sidebar:hover::-webkit-scrollbar-thumb {
    background: hsla(0,0%,53%,.4)
}

.sidebar:hover::-webkit-scrollbar-track {
    background: hsla(0,0%,53%,.1)
}
```

<br/>

#### 二、滚动条相关伪类

定义滚动条就是利用**伪元素**与**伪类**相互作用

```css
:horizontal //适用于任何水平方向上的滚动条
:vertical //适用于任何垂直方向的滚动条
:decrement //适用于按钮和轨道碎片。表示递减的按钮或轨道碎片
:increment //适用于按钮和轨道碎片。表示递增的按钮或轨道碎片
:start //适用于按钮和轨道碎片。表示对象（按钮轨道碎片）是否放在滑块的前面
:end //适用于按钮和轨道碎片。表示对象（按钮轨道碎片）是否放在滑块的后面
:double-button //适用于按钮和轨道碎片。判断轨道结束的位置是否是一对按钮。
:single-button //适用于按钮和轨道碎片。判断轨道结束的位置是否是一个按钮。
:no-button //表示轨道结束的位置没有按钮。
:corner-present //表示滚动条的角落是否存在。
:window-inactive //适用于所有滚动条，表示包含滚动条的区域，焦点不在该窗口的时候。
```

```css
::-webkit-scrollbar-track-piece:start {
    /*滚动条上半边或左半边*/
}
::-webkit-scrollbar-thumb:window-inactive {
    /*当焦点不在当前区域滑块的状态*/
}
::-webkit-scrollbar-button:horizontal:decrement:hover {
    /*当鼠标在水平滚动条下面的按钮上的状态*/
}
```

<br/>

#### 三、IE滚动条

![](CSS-自定义webkit滑动条\1.png)

```css
body {
    scrollbar-base-color: #C0C0C0;
    scrollbar-base-color: #C0C0C0;
    scrollbar-3dlight-color: #C0C0C0;
    scrollbar-highlight-color: #C0C0C0;
    scrollbar-track-color: #EBEBEB;
    scrollbar-arrow-color: black;
    scrollbar-shadow-color: #C0C0C0;
    scrollbar-dark-shadow-color: #C0C0C0;
}
```

注：IE只能修改滚动条的颜色

<br/>

