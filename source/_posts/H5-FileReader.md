---
title: H5-FileReader
date: 2017-07-25 20:57:28
tags: [FileReader,FileList,File]
---

# 一、FileList 

**FileList 对象**针对表单的 **file 控件**。当用户通过 file 控件选取文件后，这个**控件的 files 属性值就是 FileList 对象**。它在结构上类似于数组，**数组里每一个元素都是File对象**，包含用户选取的多个文件。如果 file 控件没有设置 **multiple** 属性，那么用户只能选择一个文件，FileList 对象也就只有一个元素了。 

<br/>

# 二、File 

我们看到一个 **FileList 对象**包含了我们选中的 **File 对象** 

<br/>

<!--more-->

# 三、FileReader 

用来把文件读入内存，并且读取文件中的数据。FileReader接口提供了一个异步API，使用该API可以在浏览器主线程中异步访问文件系统，读取文件中的数据。 

#### 1、FileReader接口的方法 

**FileReader接口有4个方法，其中3个用来读取文件，另一个用来中断读取**。无论读取成功或失败，方法并不会返回读取结果，这一结果存储在result属性中。 

| 方法名             | 参数                | 描述                   |
| :----------------- | ------------------- | ---------------------- |
| readAsBinaryString | file                | 将文件读取为二进制编码 |
| readAsText         | file,[**encoding**] | 将文件读取为文本       |
| readAsDataURL      | file                | 将文件读取为DataURL    |
| abort              | (none)              | 终端读取操作           |

<br/>

#### 2、FileReader接口事件 

FileReader接口包含了一套完整的事件模型，用于捕获读取文件时的状态。 

| 事件        | 描述                   |
| ----------- | ---------------------- |
| onabort     | 中断                   |
| onerror     | 出错                   |
| onloadstart | 开始                   |
| onprogress  | 正在读取               |
| onload      | 成功读取               |
| onloadend   | 读取完成，无论成功失败 |

<br/>

```javascript
var  result = document.getElementById("result");  
var  file = document.getElementById("file");    

//判断浏览器是否支持FileReader接口  
if (typeof  FileReader  ==  'undefined') {
    result.InnerHTML = "<p>你的浏览器不支持FileReader接口！</p>";  
    //使选择控件不可操作
    file.setAttribute("disabled", "disabled");  
}

//检验是否为图像文件 
function  readAsDataURL() {
    var  file  =  document.getElementById("file").files[0];
    if (!/image\/\w+/.test(file.type)) {
        alert("看清楚，这个需要图片！");
        return  false;
    }
    var  reader  =  new  FileReader();
    //将文件以Data URL形式读入页面
    reader.readAsDataURL(file);
    reader.onload = function(e) {
        //显示文件  
        var  result = document.getElementById("result");
        result.innerHTML = '<img src="'  +  this.result  + '" alt="" />';
    }
}    


function  readAsBinaryString() {
    var  file  =  document.getElementById("file").files[0];
    var  reader  =  new  FileReader();
    //将文件以二进制形式读入页面 
    reader.readAsBinaryString(file);
    reader.onload = function(f) {
        //显示文件  
        var  result = document.getElementById("result");
        result.innerHTML = this.result;
    }
}    


function  readAsText() {
    var  file  =  document.getElementById("file").files[0];
    var  reader  =  new  FileReader();
    //将文件以文本形式读入页面
    reader.readAsText(file);
    reader.onload = function(f) {
        //显示文件
        var  result = document.getElementById("result");
        result.innerHTML = this.result;
    }
}
```



```html
<p>
    <label>请选择一个文件：</label>
    <input type="file"  id="file"  />
    <input type="button"  value="读取图像"  onclick="readAsDataURL()"  />
    <input type="button"  value="读取二进制数据"  onclick="readAsBinaryString()"  />
    <input type="button"  value="读取文本文件"  onclick="readAsText()"  />
</p>
<div id="result"  name="result"></div>  
```

