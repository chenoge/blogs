---
title: css-权重
date: 2018-03-23 21:59:46
tags: [css,权重]
---

# 权重等级

根据选择器种类的不同可以分为四类，也决定了四种不同等级的权重值。

##### 0、!important（10,000）

##### 1、行内样式style（1,000）

##### 2、ID选择器（100）

##### 3、类、属性选择器、伪类选择器（10）

伪类选择器

- :hover
- :focus

##### 4、元素、伪元素（1）

伪元素选择器:

- ::after 
- ::before 
- ::first-letter 
- ::first-line 
- ::selecton

<!--more--> 



伪元素跟伪类都是选择器的补充，但是，**伪类表示的是一种“状态”**。比如hover，active等等，而**伪元素表示文档的某个确定部分的表现**，比如::first-line 伪元素只作用于你前面元素选择器确定的一个元素的第一行。 

注意，伪元素选择器选择出来的“部分”不在dom里，也不能对其绑定事件。如果你对伪元素前面的选择器定义的元素绑定了事件，伪元素同样会生效。 永远记得伪元素生成的是“表现”。

![](css-权重\priority_rules_1.jpg)



注意：

- **通配选择符**（universal selector）(*), **关系选择符**（combinators） (+, >, ~, ' ')  和 **否定伪类**（negation pseudo-class）(:not()) 对优先级没有影响。（但是，在 :not() 内部声明的选择器是会影响优先级）。 

- `@media`媒体查询相当于逻辑`if`的作用，但不影响优先级。**媒体查询主要利用属性覆盖产生效果，因此媒体查询样式的位置特别重要。**

  ```css
  @media screen and (max-width: 500px) {
      body {
          background-color:red;
      }
  }
  
  body {
      background-color:lightgreen;
  }
  
  /*如果媒体查询放在普通样式之前，样式无法进行覆盖，没有起到相应的效果*/
  ```

  