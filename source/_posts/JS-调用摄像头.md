---
title: JS-调用摄像头
date: 2018-05-08 15:39:21
tags: [javaScript,摄像]
---

# 一、用法

``` javascript
navigator.mediaDevices.getUserMedia(constraints)
    .then(function(stream) {
        /* 使用这个stream stream */
    })
    .catch(function(err) {
        /* 处理error */
    });
```

**MediaDevices.getUserMedia()**会提示用户给予使用媒体输入的许可。它返回一个 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象，成功后会resolve回调一个 [MediaStream](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStream) 对象。若用户拒绝了使用权限，或者需要的媒体源不可用，promise会reject回调一个  PermissionDeniedError 或者 NotFoundError 。 



# 二、参数Constraints

1. 以下同时请求不带任何参数的音频和视频： 

   ``` javascript
   {
       audio: true,
       video: true
   }
   ```

   

2. 当由于隐私保护的原因，无法访问用户的摄像头和麦克风信息时，应用可以使用额外的constraints参数请求它所需要或者想要的摄像头和麦克风能力。下面演示了应用想要使用1280x720的摄像头分辨率： 

   ``` javascript
   {
       audio: true,
       video:
       {
           width: 1280,
           height: 720
       }
   }
   ```

   浏览器会试着满足这个请求参数，但是如果无法准确满足此请求中参数要求或者用户选择覆盖了请求中的参数时，有可能返回其它的分辨率。 

   <!--more--> 

3. 强制要求获取特定的尺寸时，可以使用关键字min, max, 或者 exact(就是 min == max). 以下参数表示要求获取最低为1280x720的分辨率。 

   ``` javascript
   {
       audio: true,
       video:
       {
           width:
           {
               min: 1280
           },
           height:
           {
               min: 720
           }
       }
   }
   ```

   如果摄像头不支持请求的或者更高的分辨率，返回的Promise会处于rejected状态，NotFoundError作为rejected回调的参数，而且用户将不会得到要求授权的提示。 

   

4. 当请求包含一个ideal（应用最理想的）值时，这个值有着更高的权重，意味着浏览器会先尝试找到最接近指定的理想值的设定或者摄像头（如果设备拥有不止一个摄像头）。 

   ``` javascript
   {
       audio: true,
       video:
       {
           width:
           {
               min: 1024,
               ideal: 1280,
               max: 1920
           },
           height:
           {
               min: 776,
               ideal: 720,
               max: 1080
           }
       }
   }
   ```

   

5. 并不是所有的constraints 都是数字。例如, 在移动设备上面，如下的例子表示优先使用前置摄像头： 

   ```javascript
   {
       audio: true,
       video:
       {
           facingMode: "user"
       }
   }
   ```

   强制使用后置摄像头，请用： 

   ``` javascript
   {
       audio: true,
       video:
       {
           facingMode:
           {
               exact: "environment"
           }
       }
   }
   ```

   
