---
title: html-meta
date: 2018-03-7 22:01:37
tags: [html,meta]
---

# 一、name属性 

`name`**属性**主要用于描述网页，与之对应的**属性值**为`content `

content中的内容是对name填入类型的具体描述，便于搜索引擎查找以及分类信息

meta标签中name属性语法格式是：

```html
<meta name="参数" content="具体的描述"> 
```

<br/>

#### A. keywords(关键字)

说明：用于告诉搜索引擎，你网页的关键字

```html
<meta name="keywords" content="Lxxyx,博客，文科生，前端">
```

<br/>

#### B. description(网站内容的描述)

说明：用于告诉搜索引擎，你网站的主要内容。

```html
<meta name="description" content="文科生，热爱前端与编程。目前大二，这是我的前端博客">
```

<br/>

#### C. viewport(移动端的窗口)

说明：这个属性常用于设计移动端网页

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

<br/>

<!--more--> 

#### D. robots(定义搜索引擎爬虫的索引方式)

说明：robots用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。

content的参数有`all`,`none`,`index`,`noindex`,`follow`,`nofollow`。默认是`all`。

```html
<meta name="robots" content="none"> 
```

具体参数如下：

1. none : 搜索引擎将忽略此网页，等价于noindex，nofollow。
2. noindex      : 搜索引擎不索引此网页。
3. nofollow:      搜索引擎不继续通过此网页的链接索引搜索其它的网页。
4. all :      搜索引擎将索引此网页与继续通过此网页的链接索引，等价于index，follow。
5. index      : 搜索引擎索引此网页。
6. follow      : 搜索引擎继续通过此网页的链接索引搜索其它的网页。

<br/>

#### E. author(作者)

说明：用于标注网页作者

```html
<meta name="author" content="Lxxyx,841380530@qq.com">
```

<br/>

#### F. generator(网页制作软件)

说明：用于标明网页是什么软件做的

```html
<meta name="generator" content="Sublime Text3">
```

<br/>

#### G. copyright(版权)

说明：用于标注版权信息

```html
<meta name="copyright" content="Lxxyx"> 
```

<br/>

#### H. revisit-after(搜索引擎爬虫重访时间)

说明：如果页面不是经常更新，为了减轻搜索引擎爬虫对服务器带来的压力，可以设置一个爬虫的重访时间。

如果重访时间过短，爬虫将按它们定义的默认时间来访问。

```html
<meta name="revisit-after" content="7 days" > 
```

<br/>

#### I. renderer(双核浏览器渲染方式)

说明：renderer是为双核浏览器准备的，用于指定双核浏览器默认以何种方式渲染页面。

```html
<meta name="renderer" content="webkit"> //默认webkit内核
<meta name="renderer" content="ie-comp"> //默认IE兼容模式
<meta name="renderer" content="ie-stand"> //默认IE标准模式
```

<br/>

# 二、 http-equiv属性

http-equiv传回一些有用的信息，以帮助浏览器正确和精确地显示网页内容 

meta标签中http-equiv属性语法格式是：

```html
<meta http-equiv="参数" content="具体的描述">
```

<br/>

#### A. content-Type(设定网页字符集)

说明：用于设定网页字符集，便于浏览器解析与渲染页面

```html
<meta http-equiv="content-Type" content="text/html;charset=utf-8">  //旧的HTML，不推荐
<meta charset="utf-8"> //HTML5设定网页字符集的方式，推荐使用UTF-8
```

<br/>

#### B. X-UA-Compatible(浏览器采取何种版本渲染当前页面)

说明：用于告知浏览器以何种版本来渲染页面

```html
//如果安装了GCF插件，则使用GCF来渲染页面，否则使用最高版本的IE内核进行渲染
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> 
```

注：

- `http-equiv=”X-UA-Compatible”`是`IE8`的专用标记，是用来指定`IE8`浏览器模**拟某个特定版本IE浏览器的渲染方式**，以此来解决IE浏览器的兼容问题。 
- `X-UA-Compatible` 中的`IE=edge`指令，可以让IE或者调用IE内核的浏览器，使用**标准模式**渲染网页，注意这里和“Edge浏览器”无关，只是恰巧重名罢了。
- `Google Chrome Frame`(谷歌内嵌浏览器框架GCF)：使用的是Google Chrome浏览器内核来渲染，而且支持IE6、7、8等多个版本的IE浏览器 

| X-UA-Compatible值 | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| IE=5              | 让浏览器使用Quirks mode来显示，实际上是使用Internet Explorer 7 的 Quirks 模式来显示内容，这个模式和IE5非常相似。 |
| IE=edge           | 这个设置是让IE使用当前的最高版本进行文档的解析，官方文档指明，edge模式仅适用在测试环境，不建议在生产环境中使用 |
| IE=7              | 使用标准IE7来处理                                            |
| IE=EmulateIE7     | 模拟IE7来处理，遵循 <!DOCTYPE> 指令，如果文档有当前有一个合法的<!DOCTYPE>，就使用IE7模式，否者使用Quirks模式（Internet Explorer 5 Quirks），对于大部分网站来说，这是首选的兼容性模式 |
| IE=8              | 标准IE8                                                      |
| IE=EmulateIE8     | 模拟IE8，遵循 <!DOCTYPE> 指令，参照IE=EmulateIE7说明         |
| IE=9              | 标准IE9                                                      |
| IE=EmulateIE9     | 模拟IE9，遵循 <!DOCTYPE> 指令，参照IE=EmulateIE7说明         |
| chrome=1          | 强制使用Chrome，需要IE下Chrome插件支持                       |
| IE=EmulateIE10    | 模拟IE10                                                     |
| IE=10             | 标准IE10，遵循 <!DOCTYPE> 指令，参照IE=EmulateIE7说明        |

<br/>

#### C. cache-control(指定请求和响应遵循的缓存机制)

说明：指导浏览器如何缓存某个响应以及缓存多长时间

 

![](D:\littleDemo\blogs\source\_posts\html-meta\1.png)

 

```html
  <meta http-equiv="cache-control" content="no-cache"> 
```

共有以下几种用法：

- no-cache:      先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。 

- no-store:      不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施） 

- public      : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果 

- private      : 只为单个用户缓存，因此不允许任何中继进行缓存。 

- maxage      : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。

例如：max-age=60表示响应可以再缓存和重用 60 秒。

<br/>

#### D. expires(网页到期时间)

说明:用于设定网页的到期时间，过期后网页必须到服务器上重新传输。

```html
<meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" /> 
```

<br/>

#### E. refresh(自动刷新并指向某页面)

说明：网页将在设定的时间内，自动刷新并调向设定的网址。

```html
//意思是2秒后跳转向我的博客
<meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/"> 
```

<br/>

#### F. Set-Cookie(cookie设定)

说明：如果网页过期。那么这个网页存在本地的cookies也会被自动删除。

```html
<meta http-equiv="Set-Cookie" content="name, date"> //格式
<meta http-equiv="Set-Cookie" content="path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT">
```





