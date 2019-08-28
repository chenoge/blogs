---
title: EJS-模板引擎
date: 2019-08-28 20:21:08
tags: [ejs]
---

#### 模板语法

- `<% Javascript代码 %>` ：`JavaScript`代码

- `<%= 变量名 %> `：将`HTML`转义的值输出到模板中
- `<%- 变量名 %>` ： 将未转义的值输出到模板中
- `<%_ 变量名  _%>` ：去除所有空格
- `<%# 注释内容 %>` ： 注释
- `<% include 文件的路径 %>` ： 引入外部的文件

<!--more-->

<br/>



#### 渲染方法

```js
let template = ejs.compile(str, options);
template(data);
// => Rendered HTML string

ejs.render(str, data, options);
// => Rendered HTML string

ejs.renderFile(filename, data, options, function(err, str){
    // str => Rendered HTML string
});
```

<br/>



##### [Options配置项](https://ejs.co/#install)

- `cache` Compiled functions are cached, requires `filename`
- `filename` Used by `cache` to key caches, and for includes
- `context` Function execution context
- `compileDebug` When `false` no debug instrumentation is compiled
- `client` Returns standalone compiled function
- `delimiter` Character to use with angle brackets for open/close
- `debug` Output generated function body
- `_with` Whether or not to use `with() {}` constructs. If `false` then the locals will be stored in the `locals` object.
- `localsName` Name to use for the object storing local variables when not using `with` Defaults to `locals`
- `rmWhitespace` Remove all safe-to-remove whitespace, including leading and trailing whitespace. It also enables a safer version of `-%>` line slurping for all scriptlet tags (it does not strip new lines of tags in the middle of a line).
- `escape` The escaping function used with `<%=` construct. It is used in rendering and is `.toString()`ed in the generation of client functions. (By default escapes XML).
- `outputFunctionName` Set to a string (e.g., `'echo'` or `'print'`) for a function to print output inside scriptlet tags.
- `async` When `true`, EJS will use an async function for rendering. (Depends on async/await support in the JS runtime.

<br/>

