---
title: 第三方Cookie
date: 2019-09-26 19:55:23
tags: [P3P,Cookie,跨域,第三方]
---

#### 第三方 Cookie

在访问**网站A**时，网站A算作**第一方**，如果网站A中引用了另一个**（域名不同的）网站X**的资源，这时这个网站X就被认为是**第三方**。**图像、 JavaScript 和 iframe** 通常会导致第三方Cookie的生成。



##### 浏览器中第三方Cookie

- `IE`：默认不接受第三方 Cookie，不过采用 **P3P** 协议即可
- `Chrome、Firefox`：默认接受，但用户可以选择不接受
- `Safari`：默认会阻止第三方cookie， `Safari `认为**相同父域名**的站点不属于第三方

<!--more-->



##### 百度统计对cookie的使用

百度统计是用的`第一方cookie+第三方cookie`的方式收集信息的。

| 名称           | 用途                                                         | 有效期 | 类型                         |
| -------------- | ------------------------------------------------------------ | ------ | ---------------------------- |
| HMACCOUNT      | Visitor Identifier，全局唯一                                 | 永久   | 第三方Cookie，hm.baidu.com域 |
| Hm_lvt_siteid  | 记录访客当前访问序列的开始时间，如果没有设置这个cookie，则访客为新访客。当本次访问是一个新的访问开始时，更新该cookie为当前时间。 | 1年    | 第一方Cookie                 |
| Hm_lpvt_siteid | 当前浏览页面时的时间，每次浏览时设置该cookie为当前时间。     | 1年    | 第一方Cookie                 |

在百度统计中，以下三条任意一个条件成立，则认为是一个新访次。

- 流量来源（referer）为非本站
- Hm_lpvt_siteid为空
- 服务器端进行计算，一个visit超过30分钟没有流量，结束当前访次



#### P3P

直接打开**页面C**会在本地保留一个`Cookie`文件，而当采用**A网站**`iframe`框架形式嵌套后就无法成功生成`Cookie`文件，该问题仅在**IE浏览器**环境下出现，`firefox、chrome、Safari`浏览器下没有问题。

造成该问题的原因：一个所谓的**隐私首选项**（简称为`P3P`）的W3C标准。只有在每一页上设置一个`Cookie`发送头，才能允许`Internet Explorer`接受第三方Cookie。

```http
P3p: CP="CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR"
```

```
PHP：header(‘P3P:CP=”IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT”‘);
```

