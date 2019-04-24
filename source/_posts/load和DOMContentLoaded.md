---
title: load和DOMContentLoaded
date: 2019-04-24 10:00:08
tags: [load,DOMContentLoaded]
---

#### load

当一个资源及其依赖资源已完成加载时，将触发load事件

<br/>



#### DOMContentLoaded

当初始的 **HTML** 文档被完全加载和解析完成之后，**DOMContentLoaded** 事件被触发，而无需等待样式表、图像和子框架的完成加载。

注意：**DOMContentLoaded** 事件必须等待其所属script之前的样式表加载解析完成才会触发。

```php
// css.php
<?php
    sleep(3);   
?>
```

```html
<link rel="stylesheet" href="css.php">
<script>
    document.addEventListener('DOMContentLoaded', function() {
        console.log('3 seconds passed');
    });
</script>
<!-- 同步 JavaScript 会暂停 DOM 的解析 -->
<!-- 如果将link置于script之后，就会立即打印 -->
```

<!--more-->

<br/>



#### 阻塞解析

1、在`Dom解析`过程中，如果遇到`<script>`标签的时候，便会停止解析，转而去处理脚本。

2、如果脚本是内联的，浏览器会先去执行这段内联的脚本，如果是外链的，那么先会去加载脚本，然后执行。在处理完脚本之后，浏览器便继续解析HTML文档。

3、同时`javascript`的执行会受到标签前面**样式文件**的影响。如果在标签前面有样式文件，需要样式文件加载并解析完毕后才执行脚本。这是因为`javascript`可以查询对象的样式。

<br/>