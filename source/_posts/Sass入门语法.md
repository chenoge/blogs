---
title: Sass入门语法
date: 2018-04-25 18:54:20
tags: [sass]
---

# 一、变量

```scss
$fontStack : Helvetica,sans-serif;
$primaryColor: #333;
body {
    font-family: $fontStack;
    color: $primaryColor;
}

/*编译结果*/
body {
    font-family: Helvetica, sans-serif;
    color: #333;
}
```

<br/>

# 二、嵌套

```scss
nav {
    ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    li {
        display: inline-block;
    }

    a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
    }
}

/*编译结果*/
nav ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

nav li {
    display: inline-block;
}

nav a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
}
```

<br/>

<!--more--> 

# 三、mixin

```scss
@mixin box-sizing ($sizing) {
    -webkit-box-sizing: $sizing;
    -moz-box-sizing: $sizing;
    box-sizing: $sizing;
}

.box-border {
    border: 1px solid #ccc;
    @include box-sizing(border-box);
}

/*编译结果*/
.box-border {
    border: 1px solid #ccc;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
```

在引用混合样式的时候，可以先将一段代码导入到混合指令中，然后再输出混合样式，额外导入的部分将出现在 @content 标志的地方： 

```scss
@mixin apply-to-ie6-only {
    * html {
        @content;
    }
}

@include apply-to-ie6-only {
    #logo {
        background-image: url(/logo.gif);
    }
}
```

**为便于书写，@mixin 可以用 = 表示，而 @include 可以用 + 表示**，所以上面的例子可以写成： 

```scss
=apply-to-ie6-only
	* html
		@content


+apply-to-ie6-only
	#logo
		background-image: url(/logo.gif)
```

注意： 当 @content 在指令中出现过多次或者出现在循环中时，额外的代码将被导入到每一个地方。 

<br/>

# 四、扩展/继承

```scss
.message {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}

.success {
    @extend .message;
    border-color: green;
}

.error {
    @extend .message;
    border-color: red;
}

.warning {
    @extend .message;
    border-color: yellow;
}

/*编译结果*/
.message, .success, .error, .warning {
    border: 1px solid #cccccc;
    padding: 10px;
    color: #333;
}

.success {
    border-color: green;
}

.error {
    border-color: red;
}

.warning {
    border-color: yellow;
}
```

<br/>

# 五、运算

```scss
.container {
    width: 100%;
}

article[role="main"] {
    float: left;
    width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
    float: right;
    width: 300px / 960px * 100%;
}

/*编译结果*/
.container {
    width: 100%;
}

article[role="main"] {
    float: left;
    width: 62.5%;
}

aside[role="complimentary"] {
    float: right;
    width: 31.25%;
}
```

