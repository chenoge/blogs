---
title: URL-createObjectURL
date: 2017-12-11 17:58:58
tags: [url,createObjectURL]
---

# createObjectURL 

`URL.createObjectURL()`方法会根据传入的参数**创建一个指向该参数对象的URL**。这个**URL的生命仅存在于它被创建的这个文档里**。新的对象**URL指向执行的File对象或者是Blob对象**。

``` javascript
objectURL = URL.createObjectURL(blob || file); 
```

- File对象，就是**一个文件**。比如我用`input type="file"`标签来上传文件,那么里面的每个文件都是一个File对象。
- Blob对象，就是**二进制数据**。比如通过new Blob()创建的对象就是Blob对象。又比如，在XMLHttpRequest里,如果指定responseType为blob，那么得到的返回值也是一个blob对象。

#### 注意：

每次调用`createObjectURL`的时候，一个新的URL对象就被创建了。即使你已经为同一个文件创建过一个URL。 **如果你不再需要这个对象，要释放它，需要使用`URL.revokeObjectURL()`方法。****当页面被关闭，浏览器会自动释放它**，但是为了最佳性能和内存使用，当确保不再用得到它的时候,就应该释放它。

<br/>

