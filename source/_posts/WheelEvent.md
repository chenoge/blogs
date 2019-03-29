---
title: WheelEvent
date: 2019-03-29 13:56:48
tags: [wheel,mousewheel,WheelEvent]
---

#### WheelEvent

`WheelEvent` DOM事件会在用户滚动鼠标滚轮或操作其它类似鼠标的设备时触发，继承了父接口`MouseEvent`、`UIEvent`、`Event`的属性。

- `WheelEvent.deltaX`只读，返回`double`值，该值表示滚轮的横向滚动量。

- `WheelEvent.deltaY`只读，返回`double`值，该值表示滚轮的纵向滚动量。

- `WheelEvent.deltaZ`只读，返回`double`值，该值表示滚轮的z轴方向上的滚动量。

- `WheelEvent.deltaMode`只读，返回`unsigned long`值，该值表示上述各delta的值的单位。

  该值及所表示的单位如下：

  | 常量              | 值     | 描述             |
  | :---------------- | ------ | ---------------- |
  | `DOM_DELTA_PIXEL` | `0x00` | 滚动量单位为像素 |
  | `DOM_DELTA_LINE`  | `0x01` | 滚动量单位为行   |
  | `DOM_DELTA_PAGE`  | `0x02` | 滚动量单位为页   |

<!--more-->

<br/>



#### 非标准事件

早期的浏览器实现过`MouseWheelEvent`和`MouseScrollEvent`两种滚轮事件接口

<br/>



##### MouseWheelEvent属性

`wheelDelta`返回`long`值，该值表示滚动的距离，以像素为单位

| 事件类型         | 事件对象         | 是否标准      | 兼容性                 |
| ---------------- | ---------------- | ------------- | ---------------------- |
| `mousewheel`     | MouseWheelEvent  | 非标准        | 只有`Firefox`不支持    |
| `DOMMouseScroll` | MouseScrollEvent | 非标准        | 只有`Firefox`支持      |
| `wheel`          | WheelEvent       | `DOM Level 3` | `Firefox 17+` 、`ie9+` |

<br/>



![](WheelEvent\1.png)

<br/>

