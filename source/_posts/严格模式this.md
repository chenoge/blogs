---
title: 严格模式this
date: 2017-12-23 20:59:17
tags: [严格模式,this]
---

0. #### 正常模式

   this的指向只有函数执行的时候才能确定，指向调用它的对象

   <br/>

1. #### 全局作用域中的this

   在严格模式下，在**全局作用域**中，this指向window对象

   <br/> 

2. #### 全局作用域中函数中的this

   在严格模式下，这种**函数中的this等于undefined**

   <br/> 

3. #### 对象的函数（方法）中的this

   在严格模式下，对象的函数中的this指向**调用函数的对象实例**

   <br/> 

4. #### 构造函数的this

   在严格模式下，构造函数中的this指向**构造函数创建的对象实例**。

   <br/> 

5. #### 事件处理函数中的this

   在严格模式下，在事件处理函数中，this指向**触发事件的目标对象**。

   <br/>

6. #### 内联事件处理函数中的this 

   在严格模式下，在内联事件处理函数中，有以下两种情况： 

   ```html
   <button onclick="alert((function(){'use strict'; return this})());">
       内联事件处理1
   </button>
   <!-- 警告窗口中的字符为undefined -->
   	
   	    
   <button onclick="'use strict'; alert(this.tagName.toLowerCase());">
       内联事件处理2
   </button>
   <!-- 警告窗口中的字符为button -->
   ```