---
title: 编写一个简单的JavaScript模板引擎
date: 2018-07-05 20:53:32
tags: [javaScript,模板引擎]
---

#### 模板

```html
<ul>
    <% for ( var i = 0; i < users.length; i++ ) { %>
         <li><a href="<%=users[i].url%>"><%=users[i].name%></a></li>
    <% } %>
</ul>
```

<br/>

<!--more-->

#### JS数据

```javascript
const arr = [
    {
        "name": "google",
        "url": "https://www.google.com"
    },
    {
        "name": "baidu",
        "url": "https://www.baidu.com/"
    },
    {
        "name": "凯斯",
        "url": "https://www.zhihu.com/people/Uncle-Keith/activities"
    }
]
const html = tmpl('list', arr)
console.log(html)
```

<br/>

#### 打印出的结果为

```html
"<ul>
    <li><a href="https://www.google.com">google</a>
    </li>
    <li><a href="https://www.baidu.com/">baidu</a>
    </li>
    <li><a href="https://www.zhihu.com/people/Uncle-Keith/activities">凯斯</a>
    </li>
</ul>"
```

<br/>

从以上的代码可以看出，将结构和数据传入tmpl函数中，就能实现拼接。而tmpl正是我们所说的模板引擎（函数）。接下来我们就来实现一下这个函数。  

<br/> 

#### 实现

模板引擎函数实现的本质，就是将**模板中HTML结构与JavaScript语句、变量分离，通过Function构造函数 + apply(call)动态生成具有数据性的HTML代码。**而如果要考虑性能的话，可以将模板进行**缓存处理。** 

实现一个模板引擎函数，大致有以下步骤：

1. **模板获取**
2. **模板中HTML结构与JavaScript语句、变量分离**
3. **Function + apply(call)动态生成JavaScript代码**
4. **模板缓存**

<br/>

1. **模板获取**

一般情况下，我们会把模板写在script标签中，赋予id属性，标识模板的唯一性；赋予type='text/html'属性，标识其MIME类型为HTML，如下

```html
<script type="text/html" id="template">
	<ul>
		<% if (obj.show) { %>
			<% for (var i = 0; i < obj.users.length; i++) { %>
				<li>
					<a href="<%= obj.users[i].url %>">
						<%= obj.users[i].name %>
					</a>
				</li>
			<% } %>
		<% } else { %>
			<p>不展示列表</p>
		<% } %>
	</ul>
</script>
```

在模板引擎中，选用`<% xxx %>`标识JavaScript语句，主要用于流程控制，无输出；`<%= xxx %>`标识JavaScript变量，用于将数据输出到模板；其余部分都为HTML代码。（与EJS类似）。 

**注：如果是模板放在一个独立的文件，可以先获取`<script>`的`scr`属性，再`ajax`请求获取文件内容。**

<br/>

2. **HTML结构与JavaScript语句、变量分离** 

主要是通过replace函数替换实现的。说明一下主要流程：

1. **创建数组arr，再拼接字符串arr.push('**
2. **遇到换行回车，替换为空字符串**
3. **遇到<%时，替换为');**
4. **遇到>%时，替换为arr.push('**
5. **遇到<%= xxx %>，结合第3、4步，替换为'); arr.push(xxx); arr.push('**
6. **最后拼接字符串'); return p.join('');**

 在代码中，需要将第5步写在2、3步骤前面，因为有**更高的优先级**，否则会匹配出错。如下 

<br/> 

```javascript
let tpl = ''
const tmpl = (str, data) => {
  // 如果是模板字符串，会包含非单词部分（<, >, %,  等）；如果是id，则需要通过getElementById获取
  if (!/[\s\W]/g.test(str)) {
      tpl = document.getElementById(str).innerHTML
  } else {
      tpl = str
  }
  let result = `let p = []; p.push('`
  result += `${
	tpl.replace(/[\r\n\t]/g, '')
	   .replace(/<%=\s*([^%>]+?)\s*%>/g, "'); p.push($1); p.push('")
	   .replace(/<%/g, "');")
	   .replace(/%>/g, "p.push('")
  }`
  result += "'); return p.join('');"      
}
```

如果模板中出现了单引号，那会影响整个函数的执行的。还有一点，如果出现了 \ 反引号，会将单引号转义了。所以需要对单引号和反引号做一下优化处理。

1. 模板中遇到 \ 反引号，需要转义
2. 遇到 ' 单引号，需要将其转义

转换为代码，即为

```javascript
str.replace(/\\/g, '\\\\')
    .replace(/'/g, "\\'")
```

```javascript
let tpl = ''
const tmpl = (str, data) => {
  // 如果是模板字符串，会包含非单词部分（<, >, %,  等）；如果是id，则需要通过getElementById获取
  if (!/[\s\W]/g.test(str)) {
      tpl = document.getElementById(str).innerHTML
  } else {
      tpl = str
  }
  let result = `let p = []; p.push('`
  result += `${
	tpl.replace(/[\r\n\t]/g, '')
           .replace(/\\/g, '\\\\')
           .replace(/'/g, "\\'")
	   .replace(/<%=\s*([^%>]+?)\s*%>/g, "'); p.push($1); p.push('")
	   .replace(/<%/g, "');")
	   .replace(/%>/g, "p.push('")
  }`
  result += "'); return p.join('');"      
} 
```

```html
<!DOCTYPE html>
<html>
<head>
    <title>Template</title>
</head>
<body>

    <div id="results"></div>

    <script type="text/html" id="user_tmpl">
        <ul>
            <% for ( var i = 0; i < users.length; i++ ) { %>
            <li><a href="<%=users[i].url%>"><%=users[i].name%></a></li>
            <% } %>
        </ul>
    </script>

    <script type="text/javascript">
        var results = document.getElementById("results");
        var users=[
            {"name":"Byron", "url":"http://localhost"},
            {"name":"Casper", "url":"http://localhost"},
            {"name":"Frank", "url":"http://localhost"}
        ];

        function tmpl(id,data){
            var html=document.getElementById(id).innerHTML;
            var result="var p=[];with(obj){p.push('"
                +html.replace(/[\r\n\t]/g," ")
                .replace(/<%=(.*?)%>/g,"');p.push($1);p.push('")
                .replace(/<%/g,"');")
                .replace(/%>/g,"p.push('")
                +"');}return p.join('');";
            var fn=new Function("obj",result);
            return fn(data);
        }

        results.innerHTML = tmpl("user_tmpl", users);
    </script>
</body>
</html>
```

