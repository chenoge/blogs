---
title: P3P实现浏览器跨域iframe读写cookies
date: 2019-09-26 19:55:23
tags: [P3P,cookies,跨域]
---

直接打开**页面C**会在本地保留一个`cookie`文件，而当采用**A网站**`iframe`框架形式嵌套后就无法成功生成`cookie`文件，该问题仅在**IE浏览器**环境下出现，`firefox、chrome、Safari`浏览器下没有问题。

造成该问题的原因：一个所谓的**隐私首选项**（简称为`P3P`）的W3C标准。只有在每一页上设置一个`cookie`发送头，才能允许`Internet Explorer`接受第三方Cookie。

```http
P3p: CP="CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR"
```

```
PHP：header(‘P3P:CP=”IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT”‘);
```

