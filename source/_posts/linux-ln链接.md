---
title: linux-ln链接
date: 2018-04-30 17:04:03
tags: [linux,ln]
---

#### 一、使用方式

```
ln [option] source_file dist_file
```

- `-f `建立时，将同档案名删除
- `-i` 删除前进行询问

``` shell
#建立abc的软连接
ln -s abc cde

#建立abc的硬连接
ln abc cde
```

<!--more-->

<br/>



#### 二、软链接与硬链接的区别

硬链接可认为是一个文件拥有两个文件名，而软链接则是系统新建一个链接文件，此文件指向其所要指的文件 

```shell
ln -s /root/lntest/source/ /root/lntest/dist/
```

（1）软连接可以跨文件系统 ，硬连接不可以 。实践的方法就是用共享文件把windows下的 aa.txt文本文档连接到linux下/root目录下。ln -s  aa.txt  /root连接成功 。ln  aa.txt  /root失败 。 



（2）关于 I节点的问题 。硬连接不管有多少个，都指向的是同一个I节点，会把 结点连接数增加，只要结点的连接数不是 0，文件就一直存在，不管你删除的是源文件还是 连接的文件 。只要有一个存在 ，文件就存在（其实也不分什么源文件连接文件的 ，因为他们指向都是同一个 I节点）。 当你修改源文件或者连接文件任何一个的时候 ，其他的文件都会做同步的修改。



（3）软链接不直接使用i节点号作为文件指针,而是使用文件路径名作为指针。所以删除连接文件对源文件无影响，但是删除源文件，连接文件就会找不到要指向的文件 。软链接有自己的inode,并在磁盘上有一小片空间存放路径名. 

<br/>



#### 三、删除链接

```cmd
# 正确
rm -rf hb_link

# 错误
rm -rf hb_link/ #删除目录下的文件和子目录内文件
```

<br/>

​                             