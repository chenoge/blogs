---
title: html-不使用table布局
date: 2018-04-09 20:56:15
tags: [html,table]
---

- Table要比其它html标记占更多的字节 



- Tablle会阻挡浏览器渲染引擎的渲染顺序
  - **table是中的内容是自适应的，它要计算嵌套最深的节点以满足自适应，所以会出现空白后才显示内容。** 



- 在某些浏览器中Table里的文字的拷贝会出现问题。 



- Table会影响其内部的某些布局属性的生效(比如<td>里的元素的height:100%) 



- table对对于页面布局来说，从语义上看是不正确的。