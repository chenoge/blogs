---
title: cursor样式
date: 2018-08-03 15:03:33
tags: [cursor]
---

# cursor样式

| 值            | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| url           | 需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| default       | 默认光标（通常是一个箭头）                                   |
| auto          | 默认。浏览器设置的光标。                                     |
| crosshair     | 光标呈现为十字线。                                           |
| pointer       | 光标呈现为指示链接的指针（一只手）                           |
| move          | 此光标指示某对象可被移动。                                   |
| e-resize      | 此光标指示矩形框的边缘可被向右（东）移动。                   |
| ne-resize     | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。          |
| nw-resize     | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。          |
| n-resize      | 此光标指示矩形框的边缘可被向上（北）移动。                   |
| se-resize     | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。          |
| sw-resize     | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。          |
| s-resize      | 此光标指示矩形框的边缘可被向下移动（南）。                   |
| w-resize      | 此光标指示矩形框的边缘可被向左移动（西）。                   |
| text          | 此光标指示文本。                                             |
| vertical-text | 用于标示可编辑的垂直文本的光标。通常是大写字母 I 旋转90度的形状。 |
| wait          | 此光标指示程序正忙（通常是一只表或沙漏）。                   |
| progress      | 带有沙漏标记的箭头光标。用于标示一个进程正在后台运行。       |
| help          | 此光标指示可用的帮助（通常是一个问号或一个气球）。           |
| all-scroll    | 有上下左右四个箭头，中间有一个圆点的光标。用于标示页面可以向上下左右任何方向滚动。 |
| col-resize    | 有左右两个箭头，中间由竖线分隔开的光标。用于标示项目或标题栏可以被水平改变尺寸。 |
| row-resize    | 有上下两个箭头，中间由横线分隔开的光标。用于标示项目或标题栏可以被垂直改变尺寸。 |
| no-drop       | 带有一个被斜线贯穿的圆圈的手形光标。用于标示被拖起的对象不允许在光标的当前位置被放下。 |
| not-allowed   | 禁止标记(一个被斜线贯穿的圆圈)光标。用于标示请求的操作不允许被执行。 |

<!--more-->

<br/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>cursor</title>
    <style>
        table,th,td {border: 1px solid #ccc;}
        thead tr {background-color: #eee;}
        tbody tr:nth-of-type(even) {background-color: #f8f8f8;} 
    </style>
</head>
<body>
<table cellpadding="6" cellspacing="0">
    <thead>
        <tr>
            <th>值</th>
            <th>描述</th>
        </tr>
    </thead>
    <tbody>
        <tr style="cursor: url;">
            <td>url</td>
            <td>需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。</td>
        </tr>
        <tr style="cursor: default;">
            <td>default</td>
            <td>默认光标（通常是一个箭头）</td>
        </tr>
        <tr style="cursor: auto;">
            <td>auto</td>
            <td>默认。浏览器设置的光标。</td>
        </tr>
        <tr style="cursor: crosshair;">
            <td>crosshair</td>
            <td>光标呈现为十字线。</td>
        </tr>
        <tr style="cursor: pointer;">
            <td>pointer</td>
            <td>光标呈现为指示链接的指针（一只手）</td>
        </tr>
        <tr style="cursor: move;">
            <td>move</td>
            <td>此光标指示某对象可被移动。</td>
        </tr>
        <tr style="cursor: e-resize;">
            <td>e-resize</td>
            <td>此光标指示矩形框的边缘可被向右（东）移动。</td>
        </tr>
        <tr style="cursor: ne-resize;">
            <td>ne-resize</td>
            <td>此光标指示矩形框的边缘可被向上及向右移动（北/东）。</td>
        </tr>
        <tr style="cursor: nw-resize;">
            <td>nw-resize</td>
            <td>此光标指示矩形框的边缘可被向上及向左移动（北/西）。</td>
        </tr>
        <tr style="cursor: n-resize;">
            <td>n-resize</td>
            <td>此光标指示矩形框的边缘可被向上（北）移动。</td>
        </tr>
        <tr style="cursor: se-resize;">
            <td>se-resize</td>
            <td>此光标指示矩形框的边缘可被向下及向右移动（南/东）。</td>
        </tr>
        <tr style="cursor: sw-resize;">
            <td>sw-resize</td>
            <td>此光标指示矩形框的边缘可被向下及向左移动（南/西）。</td>
        </tr>
        <tr style="cursor: s-resize;">
            <td>s-resize</td>
            <td>此光标指示矩形框的边缘可被向下移动（南）。</td>
        </tr>
        <tr style="cursor: w-resize;">
            <td>w-resize</td>
            <td>此光标指示矩形框的边缘可被向左移动（西）。</td>
        </tr>
        <tr style="cursor: text;">
            <td>text</td>
            <td>此光标指示文本。</td>
        </tr>
        <tr style="cursor: vertical-text;">
            <td>vertical-text</td>
            <td>用于标示可编辑的垂直文本的光标。通常是大写字母 I 旋转90度的形状。</td>
        </tr>
        <tr style="cursor: wait;">
            <td>wait</td>
            <td>此光标指示程序正忙（通常是一只表或沙漏）。</td>
        </tr>
        <tr style="cursor: progress;">
            <td>progress</td>
            <td>带有沙漏标记的箭头光标。用于标示一个进程正在后台运行。</td>
        </tr>
        <tr style="cursor: help;">
            <td>help</td>
            <td>此光标指示可用的帮助（通常是一个问号或一个气球）。</td>
        </tr>
        <tr style="cursor: all-scroll;">
            <td>all-scroll</td>
            <td>有上下左右四个箭头，中间有一个圆点的光标。用于标示页面可以向上下左右任何方向滚动。</td>
        </tr>
        <tr style="cursor: col-resize;">
            <td>col-resize</td>
            <td>有左右两个箭头，中间由竖线分隔开的光标。用于标示项目或标题栏可以被水平改变尺寸。</td>
        </tr>
        <tr style="cursor: row-resize;">
            <td>row-resize</td>
            <td>有上下两个箭头，中间由横线分隔开的光标。用于标示项目或标题栏可以被垂直改变尺寸。</td>
        </tr>
        <tr style="cursor: no-drop;">
            <td>no-drop</td>
            <td>带有一个被斜线贯穿的圆圈的手形光标。用于标示被拖起的对象不允许在光标的当前位置被放下。</td>
        </tr>
        <tr style="cursor: not-allowed;">
            <td>not-allowed</td>
            <td>禁止标记(一个被斜线贯穿的圆圈)光标。用于标示请求的操作不允许被执行。</td>
        </tr>
    </tbody>
</table>
</body>
</html>
```

