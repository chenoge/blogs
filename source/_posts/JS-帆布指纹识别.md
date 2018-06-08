---
title: JS-帆布指纹识别
date: 2018-04-14 21:45:39
tags: [javaScript,指纹识别]
---

# 一、利用canvas标签

一般情况下，网站或者广告联盟都会非常想要一种技术方式可以**在网络上<font color=#A52A2A size=4 >精确定位</font>到每一个个体**，这样可以通过收集这些个体的数据，通过分析后更加精准的去推送广告（精准化营销）或其他有针对性的一些活动。<br/>

Cookie技术是非常受欢迎的一种。当用户访问一个网站时，网站可以在用户当前的浏览器Cookie中永久植入一个含有唯一标示符（UUID）的信息，并通过这个信息将用户所有行为关联起来。<br/>

帆布指纹识别使用到了HTML5<canvas>标签的一个特点：<font color=#A52A2A size=4 >**在绘制canvas图片时，同样的canvas绘制代码，不同机器和浏览器绘制的图片特征是相同并且独一无二的**</font>，这样以来，提取最简单的md5值便可以唯一标识和跟踪这个用户。

```javascript
  var canvas = document.createElement('canvas');
  var ctx = canvas.getContext('2d');
  var txt = 'http://security.tencent.com/';
  ctx.textBaseline = "top";
  ctx.font = "14px 'Arial'";
  ctx.textBaseline = "tencent";
  ctx.fillStyle = "#f60";
  ctx.fillRect(125,1,62,20);
  ctx.fillStyle = "#069";
  ctx.fillText(txt, 2, 15);
  ctx.fillStyle = "rgba(102, 204, 0, 0.7)";
  ctx.fillText(txt, 4, 17);
  var b64 = canvas.toDataURL().replace("data:image/png;base64,","");
```

<br/>

# 二、配合navigator和screen等属性

- `navigator.userAgent`
- `navigator.mimeTypes`
- `navigator.plugins`
- `navigator.language`
- `screen.height`
- `screen.width`
- `screen.colorDepth`
- `window.devicePixelRatio`
- 检测已安装的字体种类

注:使用hash获取字符标识，封装得比较好的框架 <font color=#A52A2A size=4 >**fingerprintjs**</font>

<br/>

# 三、应用

除了可以**追踪用户习惯**，还可以用来**防止用户信息受XSS攻击影响**。一般用户的登录状态使用cookie记录，如果被黑客使用XSS攻击或者其他途径获取，可以使用**UUID和cookie来做双重验证**。