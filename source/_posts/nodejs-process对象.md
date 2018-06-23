---
title: nodejs-process对象
date: 2017-08-05 15:04:26
tags: [nodejs,process]
---

process是Node.js提供的一个对象，它代表当前Node.js进程。

```javascript
process === global.process;	//True
	
process.version;	//'v5.2.0'
	
process.platform;	//'darwin'
	
process.arch;	//'x64'
	
process.cwd();	//返回当前工作目录:	'/Users/michael'
	
process.chdir('/private/tmp'); // 切换当前工作目录:Undefined
	
process.cwd();	//	'/private/tmp'

```

<br/>

<!--more--> 

**JavaScript程序是由事件驱动执行的单线程模型，Node.js也不例外。**如果我们想要在下一次事件循环中执行代码，可以调用**process.nextTick()**： 

```javascript
// test.js
// process.nextTick()将在下一轮事件循环中调用:

process.nextTick(function () {
    console.log('nextTick callback!');
});

console.log('nextTick was set!');

```

用Node执行上面的代码node test.js，你会看到，打印输出是： 

```javascript
nextTick was set!
nextTick callback!
```

这说明传入process.nextTick()的函数不是立刻执行，而是要等到**下一次事件循环**。 

<br/>

Node.js进程本身的事件就由process对象来处理。如果我们响应exit事件，就可以在程序即将退出时执行某个回调函数： 

```javascript
// 程序即将退出时的回调函数:
process.on('exit', function (code) {
    console.log('about to exit with code: ' + code);
});
```

