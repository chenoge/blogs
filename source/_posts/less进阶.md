---
title: less进阶
date: 2017-12-28 19:17:35
tags: [less]
---

# 一、变量 

1. 变量定义方式是 <font color=#A52A2A size=4 >     **@变量名:值**</font>
2. 变量使用方式是 <font color=#A52A2A size=4 >     **@{变量名} || @变量名**</font>

```less
	@kuandu:width;
	@green: #801f77;
	@imgurl:"https://www.baidu.com/img/";
	.@{kuandu}{
		@{kuandu}:150px;
		//使用””将变量的值括起来
		background: @green url("@{imgurl}bdlogo.png");
	}
	
	/*编译后*/
	.width {
	  width: 150px;
	  background: #801f77 url("https://www.baidu.com/img/bdlogo.png");
	}
```

注：

1. 变量作为<font color=#A52A2A size=4 > **属性名</font>**时 **@{变量名}**
2. 变量作为<font color=#A52A2A size=4 > **属性值**</font>时 **@变量名   ||          "@{变量名}"**

<br/>

# 二、多参数混合

1、命名参数：引用mixin时可以通过参数名称而不是参数的位置来为mixin提供参数值

```less
	.mixin(@color: black; @margin: 10px; @padding: 20px) {
	  color: @color;
	  margin: @margin;
	  padding: @padding;
	}

	.class3{
	  .mixin(@padding: 80px;)
	}

	/*编译后*/
	.class3 {
	  color: black;
	  margin: 10px;
	  padding: 80px;
	}
```



2、匹配模式：传值的时候定义一个字符，在使用的时候使用哪个字符，就调用那天规则

```less
	.border(b-l,@w:5px){
	  border-bottom-left-radius: @w;
	}

	.border(b-r,@w:5px){
	  border-bottom-right-radius: @w;
	}
	
	footer{
	  .border(b-r,10px);
	  background: #33acfe;
	}

	/*编译后*/
	footer {
	  border-bottom-right-radius: 10px;
	  background: #33acfe;
	}
```



3、返回值

```less
	.average(@x, @y) {
	  @average: ((@x + @y) / 2);
	  @he:(@x + @y);
	}
	
	div {
	  //执行混合
	  .average(16px, 50px);
	  //使用返回变量
	  padding: @average;
	  margin: @he;
	}

	/*编译后*/
	div {
	  padding: 33px;
	  margin: 66px;
	}
```

<br/>

<!--more--> 

# 三、嵌套

1、父类选择器符号： & 

```less
	.logo{
		width: 300px;
		&:hover{
			background: forestgreen;
		}
	}

	.logo {
	  width: 300px;
	}

	/*编译后*/
	.logo:hover {
	  background: forestgreen;
	}
```



2、改变选择器顺序：将&放到当前选择器之后，就会将当前选择器插入到所有的父选择器之前

```less
	.a{
		.b{
			.c&{
	      color: 123;
	    }
	  }
	}
	
	/*编译后*/
	.c.a .b {
	  color: 123;
	}
```



3、组合 & &：组合使用生成所有可能的选择器列表

```less
	p, a, ul {
	  border-top: 2px dotted #366;
	  &   & {
	    border-top: 0;
	  }
	}

	p,
	a,
	ul {
	  border-top: 2px dotted #366;
	}

	/*编译后*/
	p p,
	p a,
	p ul,
	a p,
	a a,
	a ul,
	ul p,
	ul a,
	ul ul {
	  border-top: 0;
	}
```

