---
title: JS-base64图片转formData
date: 2017-11-08 18:41:01
tags: [base64,formDate]
---

```javascript
/**
 * @param base64Codes
 * 图片的base64编码
 */
function sumitImageFile(base64Codes) {
    var form = document.forms[0];
    //这里连带form里的其他参数也一起提交了,如果不需要提交其他参数可以直接FormData无参数的构造函数
    var formData = new FormData(form);
    //convertBase64UrlToBlob函数是将base64编码转换为Blob
    //append函数的第一个参数是后台获取数据的参数名,和html标签的input的name属性功能相同
    formData.append("imageName", convertBase64UrlToBlob(base64Codes));

    //ajax 提交form
    $.ajax({
        url: form.action,
        type: "POST",
        data: formData,
        dataType: "text",
        processData: false, // 告诉jQuery不要去处理发送的数据
        contentType: false, // 告诉jQuery不要去设置Content-Type请求头

        success: function(data) {
            window.location.href = "${ctx}" + data;
        },
        xhr: function() { //在jquery函数中直接使用ajax的XMLHttpRequest对象
            var xhr = new XMLHttpRequest();

            xhr.upload.addEventListener("progress", function(evt) {
                if (evt.lengthComputable) {
                    var percentComplete = Math.round(evt.loaded * 100 / evt.total);
                    console.log("正在提交." + percentComplete.toString() + '%'); 
                }
            }, false);

            return xhr;
        }

    });
}
```

<!--more-->

```javascript
/**
 * 将以base64的图片url数据转换为Blob
 * @param urlData
 * 用url方式表示的base64图片数据
 */
function convertBase64UrlToBlob(dataUrl) {
    //去掉url的头，并转换为byte
    var bytes = window.atob(dataUrl.split(',')[1]);
    //处理异常,将ascii码小于0的转换为大于0
    var ab = new Uint8Array(bytes.length);
    for (var i = 0; i < bytes.length; i++) {
        ab[i] = bytes.charCodeAt(i);
    }
    return new Blob([ab], { type: 'image/png' });
}
```

