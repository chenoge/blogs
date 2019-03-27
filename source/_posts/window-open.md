---
title: window-open
date: 2019-03-27 14:00:44
tags: [window,open]
---

```javascript
let windowObjectReference = window.open(strUrl, strWindowName, [strWindowFeatures]);
```

- `WindowObjectReference`

  打开的新窗口对象的引用。如果调用失败，返回值会是 `null` 。如果父子窗口满足`同源策略`，你可以通过这个引用访问新窗口的属性或方法

  

- `strUrl`

  新窗口需要载入的url地址。`strUrl`可以是web上的`html页面`也可以是`图片文件`或者其他任何浏览器支持的`文件格式`



- `strWindowName`

  新窗口的名称。该字符串可以用来作为超链接 `a`或表单 `form`元素的目标属性值。**字符串中不能含有空白字符**。注意：**strWindowName 并不是新窗口的标题**

  - `_blank` - URL加载到一个新的窗口。这是**默认**
  - `_parent` - URL加载到父框架
  - `_self` - URL替换当前页面
  - `_top` - URL替换任何可加载的框架集



- `strWindowFeatures`

  可选参数。是一个字符串值，这个值列出了将要打开的窗口的一些特性(窗口功能和工具栏) 。 字符串中不能包含任何空白字符，特性之间用逗号分隔开



```javascript
myWindow=window.open('','','width=200,height=100');
window.open("http://www.runoob.com");
```

<!--more-->

<br/>

注：

- `window.open`或者`a.href`不能自定义`header`
- ajax不能拦截`301/302`

<br/>