---
title: Api-RESTful
date: 2018-01-30 22:09:01
tags: [Api,RESTful]
---

# 一、资源

REST，即Representational State Transfer的缩写，资源表现层状态转化 

所谓"资源"，就是网络上的一个具体信息。你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI，因此URI就成了每一个资源的地址或独一无二的识别符。 

- "资源"表示一种实体，所以应该是名词，**URI不应该有动词**
- 数据库中的表都是**同种记录的"集合"**（collection），所以API中的**名词也应该使用复数**。 

<br/>

# 二、表现层

"资源"是一种信息实体，它可以有多种外在表现形式。我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。 

比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。 

URI只代表资源的实体，不代表它的形式。严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置。**它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。**

<br/> 

# 三、状态转化

HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。 

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
- DELETE（DELETE）：从服务器删除资源。 

<br/>

- GET      /zoos：列出所有动物园
- POST      /zoos：新建一个动物园
- GET      /zoos/ID：获取某个指定动物园的信息
- PUT      /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
- PATCH      /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
- DELETE      /zoos/ID：删除某个动物园
- GET      /zoos/ID/animals：列出某个指定动物园的所有动物
- DELETE      /zoos/ID/animals/ID：删除某个指定动物园的指定动物

 <br/> 

# 四、RESTful架构：

（1）每一个URI代表一种资源；

（2）客户端和服务器之间，传递这种资源的某种表现层；

（3）客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"

 <br/> 

# 五、状态码

- 200      OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
- 201      CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
- 202      Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
- 204      NO CONTENT - [DELETE]：用户删除数据成功。
- 400      INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
- 401      Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
- 403      Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
- 404      NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
- 406      Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
- 410      Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
- 422      Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
- 500      INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

 <br/> 

# 六、返回结果

- GET      /collection：返回资源对象的列表（数组）
- GET      /collection/resource：返回单个资源对象
- POST      /collection：返回新生成的资源对象
- PUT      /collection/resource：返回完整的资源对象
- PATCH      /collection/resource：返回完整的资源对象
- DELETE      /collection/resource：返回一个空文档