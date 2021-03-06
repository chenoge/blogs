---
title: NAT原理与穿透
date: 2019-07-14 00:34:43
tags: [NAT,穿透]
---

#### NAT原理

网络地址转换`(NAT,Network Address Translation)`属接入广域网(WAN)技术，是一种将私有（保留）地址转化为合法IP地址的转换技术。下面介绍两类不同方式实现的NAT：

1. ##### `NAT(Network Address Translators)`：称为基本的NAT

![](NAT原理与穿透\1.gif)

```
在客户机时     192.168.0.8:4000——6.7.8.9:8000
在网关时       1.2.3.4:4000——6.7.8.9:8000
服务器C        6.7.8.9:8000

其核心是替换IP地址而不是端口，这会导致192.168.0.8使用4000端口后，192.168.0.9如何处理？具体参考RFC 1631。基本上这种类型的NAT设备已经很少了。或许根本我们就没机会见到。
```

<!--more-->

<br/>



2. `NAPT(Network Address/Port Translators)`：其实这种才是我们常说的 NAT

   NAPT的特点是在网关时，会使用网关的 IP，但端口会选择一个和临时会话对应的临时端口。如下图：

   ![](NAT原理与穿透\2.gif)

   ```
   在客户机时           192.168.0.8:4000——6.7.8.9:8000
   在网关时            1.2.3.4:62000——6.7.8.9:8000
   服务器C             6.7.8.9:8000
   
   网关上建立保持了一个1.2.3.4:62000的会话，用于192.168.0.8:4000与6.7.8.9:8000之间的通讯。
   ```

对于NAPT，又分了两个大的类型，差别在于，当两个内网用户同时与8000端口通信的处理方式不同：

##### 2.1、 `Symmetric` NAT型 (对称型)

![](NAT原理与穿透\3.gif)

```
在客户机时   192.168.0.8:4000——6.7.8.9:8000 192.168.0.8:4000——6.7.8.10:8000
在网关时     1.2.3.4:62000——6.7.8.9:8000 1.2.3.4:62001——6.7.8.10:8000
服务器C      6.7.8.9:8000
服务器 D     6.7.8.10:8000

这种形式会让很多p2p软件失灵。
```

<br/>



##### 2.2、Cone NAT型（圆锥型）

![](NAT原理与穿透\4.gif)

```
在客户机时  192.168.0.8:4000——6.7.8.9:8000 192.168.0.8:4000——6.7.8.10:8000
在网关时    1.2.3.4:62000——6.7.8.9:8000 1.2.3.4:62000——6.7.8.10:8000
服务器C     6.7.8.9:8000
服务器D     6.7.8.10:8000

目前绝大多数属于这种。
```
<br/>



##### Cone NAT又分了3种类型：

```
a)Full Cone NAT（完全圆锥型）：从同一私网地址端口192.168.0.8:4000发至公网的所有请求都映射成同一个公网地址端口1.2.3.4:62000 ，192.168.0.8可以收到任意外部主机发到1.2.3.4:62000的数据报。

b)Address Restricted Cone NAT （地址限制圆锥型）：从同一私网地址端口192.168.0.8:4000发至公网的所有请求都映射成同一个公网地址端口1.2.3.4:62000，只有当内部主机192.168.0.8先给服务器C 6.7.8.9发送一个数据报后，192.168.0.8才能收到6.7.8.9发送到1.2.3.4:62000的数据报。

c)Port Restricted Cone NAT（端口限制圆锥型）：从同一私网地址端口192.168.0.8:4000发至公网的所有请求都映射成同一个公网地址端口1.2.3.4:62000，只有当内部主机192.168.0.8先向外部主机地址端口6.7.8.9：8000发送一个数据报后，192.168.0.8才能收到6.7.8.9：8000发送到1.2.3.4:62000的数据报。  
```

<br/>



#### NAT穿透

![](NAT原理与穿透\5.gif)

```
A1在客户机时                192.168.0.8:4000——6.7.8.9:8000
X1在网关时                   1.2.3.4:62000——6.7.8.9:8000

B1在客户机时                192.168.1.8:4000——6.7.8.9:8000
Y1在网关时                   1.2.3.5:31000——6.7.8.9:8000

服务器C                       6.7.8.9:8000
```

<br/>



两内网用户要实现通过各自网关的直接呼叫，需要以下过程：

```
1、 客户机A1、B1顺利通过格子网关访问服务器C ，均没有问题（类似于登录）

2、 服务器C保存了 A1、B1各自在其网关的信息（1.2.3.4:62000、1.2.3.5:31000）没有问题。并可将该信息告知A1、B2。

3、 此时A1发送给B1网关的1.2.3.5:31000是否会被B1收到？答案是基本上不行（除非Y1设置为完全圆锥型，但这种设置非常少），因为Y1上检测到其存活的会话中没有一个的目的IP或端口于1.2.3.4:62000有关而将数据包全部丢弃！

4、 此时要实现A1、B1通过X1、Y1来互访，需要服务器C告诉它们各自在自己的网关上建立“UDP隧道”，即命令A1发送一个 192.168.0.8:4000——1.2.3.5:31000的数据报，B1发送一个192.168.1.8:4000——1.2.3.4:62000的数据报，UDP形式，这样X1、Y1上均存在了IP端口相同的两个不同会话（很显然，这要求网关为Cone NAT型，否则，对称型Symmetric NAT设置网关将导致对不同会话开启了不同端口，而该端口无法为服务器和对方所知，也就没有意义）。

5、 此时A1发给Y1，或者B1发给X1的数据报将不会被丢弃且正确的被对方收到.

6.为了保证A1的路由器X1有与B1的session，A1要定时与B1做心跳包，同样，B1也要定时与A1做心跳，这样，双方的通信通道都是通的，就可以进行任意的通信了。
```

<br/>



#### UDP和TCP打洞

TCP和UDP在打洞上是不同的，这是因为伯克利socket（标准socket规范）的API造成的。

- **UDP的socket允许多个socket绑定到同一个本地端口，而TCP的socket则不允许**

注：`TCP`按`CS`方式工作，一个端口只能用来`connect`或`listen`，所以需要使用**端口重用**，才能利用本地NAT的端口映射关系。要设置`SO_REUSEADDR`、`SO_REUSEPORT`这两个参数，需要系统支持`SO_REUSEPORT`。

<br/>



#### 内网IP段

```
192.168.0.0 - 192.168.255.255
172.16.0.0 - 172.31.255.255
10.0.0.0 - 10.255.255.255
```

