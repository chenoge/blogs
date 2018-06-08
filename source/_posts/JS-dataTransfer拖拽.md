---
title: JS-dataTransfer拖拽
date: 2018-05-09 15:34:16
tags: [javaScript,dataTransfer,拖拽]
---

# 一、对象方法

``` javascript
var dsHandler = function(evt) {
    evt.dataTransfer.setData("text/plain", "<item>" + evt.target.innerHTML);
}
```

- **setData(format,data):**
  - ​		将指定格式的数据赋值给dataTransfer对象
  - 参数format定义数据的格式也就是数据的类型，data为待赋值的数据

<br/>

- **getData(format):**
  - 从dataTransfer对象中获取指定格式的数据，format代表数据格式，data为数据。

<br/>

- **clearData([format]):**
  - 从dataTransfer对象中删除指定格式的数据，参数可选。
  - 若不给出，则为删除对象中所有的数据