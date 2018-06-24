---
title: JS-三种对话框
date: 2017-07-25 20:42:02
tags: [javaScript,对话框]
---

```javascript
//弹出对话框并输出一段提示信息  
function ale() {
    //弹出一个对话框  
    alert("提示信息！");  
}

//弹出一个询问框，有确定和取消按钮  
function firm() {  
    //利用对话框返回的值 （true 或者 false）  
    if (confirm("你确定提交吗？")) {  
        alert("点击了确定");  
    }  
    else {  
        alert("点击了取消");  
    }  
}

//弹出一个输入框，输入一段文字，可以提交  
function prom() {  
    //将输入的内容赋给变量 name ，  
    var name = prompt("请输入您的名字", "");
    //这里需要注意的是，prompt有两个参数，前面是提示的话，后面是当对话框出来后，在对话框里的默认值  
    //如果返回的有内容 
    if (name) 
    {  
        alert("欢迎您：" + name)  
    }  
}  
```

