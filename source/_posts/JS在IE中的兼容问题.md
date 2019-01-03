---
title: JS在IE中的兼容问题
date: 2019-01-02 23:23:26
tags: [js,IE,兼容]
---

#### 标识符与保留字

`保留字`是`JavaScript`语言中定义`具有特殊含义的标识符`，**保留字不能作为标识符使用**。`JavaScript`语言中定义了一些`具有专门的意义和用途的保留字`，这些保留字称为`关键字`。

**标识符：变量，属性，数组，函数名称 **  

<br/>

#### SCRIPT1010

`缺少标识符` ，一般在`IE浏览器`下，使用了保留字就会报这个错误，如： `default`, `delete` 等

<br/>

#### SCRIPT1028 

```javascript
// 对象最后一项是不允许有逗号的，跟json的规则相似
let point = { x: 1.2, y: -3.4 }; // 合法声明
let point = { x: 1.2, y: -3.4, }; // 缺少标识符、字符串或数字

// 使用了保留字
let point = { x: 1.2, 'delete': -3.4 }; // 合法声明
let point = { x: 1.2,  delete: -3.4 }; // 缺少标识符、字符串或数字
```

`对象文字值的属性`必须是`标识符`、`字符串`或`数字`。 对象文字值（也称为“对象初始值设定项”）由逗号分隔的`“属性:值”`对的列表构成，其中各对都括在括号中。

注：使用打包工具会存在`'delete'`变为`delete`的情况，最好不要使用保留字作为标识符。

<br/>

#### 版本

```javascript
// 相同工程：线上环境 
// 存在以上问题
// IE11 浏览器的 navigator.userAgent
"Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; Tablet PC 2.0)"
```

注：`IE11`不存在以上问题

```javascript
// 相同工程：开发环境、vue、webpack-dev-server 
// 不存在以上问题
// IE11 浏览器的 navigator.userAgent
"Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; Tablet PC 2.0; rv:11.0) like Gecko"
```
注：项目中不曾使用`X-UA-Compatible`指定`IE内核版本` 

```html
 <meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

<br/>

#### 检测页面IE版本

```javascript
 function IEVersion() {
     //取得浏览器的userAgent字符串  
     var userAgent = navigator.userAgent;
     
     //判断是否IE<11浏览器  
     var isIE = userAgent.indexOf("compatible") > -1 && userAgent.indexOf("MSIE") > -1; 
     
     //判断是否IE的Edge浏览器
     var isEdge = userAgent.indexOf("Edge") > -1 && !isIE; 
     
     //判断是否IE11浏览器  
     var isIE11 = userAgent.indexOf('Trident') > -1 && userAgent.indexOf("rv:11.0") > -1;

     if (isEdge) {
         return 'edge'; //edge
     }

     if (isIE11) {
         return 11; //IE11  
     }

     if (isIE) {
         var reIE = new RegExp("MSIE (\\d+\\.\\d+);");
         reIE.test(userAgent);
         var fIEVersion = parseFloat(RegExp["$1"]);
         
         if (fIEVersion == 7) {
             return 7;
         }

         if (fIEVersion == 8) {
             return 8;
         }

         if (fIEVersion == 9) {
             return 9;
         }

         if (fIEVersion == 10) {
             return 10;
         }

         {
             return 6; //IE版本<=7
         }
     }

     return -1; //不是ie浏览器
 }
```

<br/>