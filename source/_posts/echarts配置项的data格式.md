---
title: echarts配置项的data格式
date: 2018-11-13 15:26:46
tags: [echarts,data]
---

#### echarts配置

##### 图例`legend`的data

- 字符串数组项

```javascript
legend: {
    data: ['邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎']
}
```



- 对象数组项

```javascript
legend: {
    data: [
        {
            name: '系列1',
            // 强制设置图形为圆。
            icon: 'circle',
            // 设置文本为红色
            textStyle: {
                color: 'red'
            }
        },
        {
            name: '系列2',
            // 强制设置图形为圆。
            icon: 'circle',
            // 设置文本为红色
            textStyle: {
                color: 'red'
            }
        }
    ]
}
```

<!--more-->

<br/>



##### 坐标轴`xAxis`与`yAxis`的data

- 字符串数组项

```javascript
xAxis: {
    type: 'category',
    data: ['周一','周二','周三','周四','周五','周六','周日']
}
```



- 对象数组项

```javascript
xAxis: {
    type: 'category',
    data: [
        {
            value: '周一',
            // 突出周一
            textStyle: {
                fontSize: 20,
                color: 'red'
            }
        }
    ]
}
```

<br/>



##### 数据`series`的data

- 通常来说，数据用一个二维数组表示

```javascript
series: [{
    data: [
        // 维度X   维度Y   其他维度 ...
        [  3.4,    4.5,   15,   43],
        [  4.2,    2.3,   20,   91],
        [  10.8,   9.5,   30,   18],
        [  7.2,    8.8,   18,   57]
    ]
}]
```


- 当某维度对应于类目轴（axis.type 为 `'category'`）的时候：其值须为类目的『**序数**』（从 `0` 开始）或者类目的『**字符串值**』

```javascript
xAxis: {
  	type: 'category',
  	data: ['星期一', '星期二', '星期三', '星期四']
},

yAxis: {
  	type: 'category',
  	data: ['a', 'b', 'm', 'n', 'p', 'q']
},

series: [{
  	data: [
  		// xAxis   yAxis
  		[   0,       0,       2], // 意思是此点位于 xAxis: '星期一', yAxis: 'a'。
  		[ '星期四',   2,       1], // 意思是此点位于 xAxis: '星期四', yAxis: 'm'。
  		[   2,      'p',      2], // 意思是此点位于 xAxis: '星期三', yAxis: 'p'。
  		[   3,       3,       5]
  	]
}]
```



- **简化**：只有一个轴为类目轴（axis.type 为 'category'）

```javascript
xAxis: {
    data: ['a', 'b', 'm', 'n']
},

series: [{
    // 与 xAxis.data 一一对应。
    data: [23,  44,  55,  19]
    // 它其实是下面这种形式的简化：
    // data: [[0, 23], [1, 44], [2, 55], [3, 19]]
}]
```



- 数组项可用对象，其中的 `value` 像表示具体的数值

  - 二维状态

    ```javascript
    series: [{
        data: [
            [12, 33],
            [34, 313],
            {
                value: [56, 44],
                label: {},
                itemStyle: {}
            },
            [10, 33]
        ]
    }]
    ```

  - 简化状态

    ```javascript
    series: [{
        data: [
            12,
            34,
            {
                value: 56,
                //自定义标签样式，仅对该数据项有效
                label: {},
                //自定义特殊 itemStyle，仅对该数据项有效
                itemStyle: {}
            },
            10
        ]
    }]
    ```

