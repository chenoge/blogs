---
title: css-响应式表单
date: 2018-03-29 22:12:36
tags: [css]
---

css3的@media检测到屏幕尺寸，将表格元素设置为block块状，并且隐藏表头，将td设置下边框看起来跟一行行的一样。最后我们使用css3的:before { content: "姓名"; }生成每行对应的标签定义，这样就能知道每行数据的意义。 

```css
	@media only screen and (max-width: 760px),
	(min-device-width: 768px) and (max-device-width: 1024px) {
	    /* Force table to not be like tables anymore */
	    table,
	    thead,
	    tbody,
	    th,
	    td,
	    tr {
	        display: block;
	    }
	    thead tr {
	        position: absolute;
	        top: -9999px;
	        left: -9999px;
	    }

	    tr {
	        border: 1px solid #ccc;
	    }

	    td {
	        /* Behave  like a "row" */
	        border: none;
	        border-bottom: 1px solid #eee;
	        position: relative;
	        padding-left: 50%;
	    }

	    td:before {
	        /* Now like a table header */
	        position: absolute;
	        /* Top/left values mimic padding */
	        top: 6px;
	        left: 6px;
	        width: 45%;
	        padding-right: 10px;
	        white-space: nowrap;
	    }

	    /*Label the data*/
	    td:nth-of-type(1):before {
	        content: "姓名";
	    }
	    td:nth-of-type(2):before {
	        content: "性别";
	    }
	    td:nth-of-type(3):before {
	        content: "出生年月";
	    }
	}
```

