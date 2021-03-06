---
title: 关于Vue单向数据流的理解
date: 2019-01-19 15:56:24
tags: [Vue,单向数据流]
---

#### 一、单向数据流

所有的 prop 都使得其`父子 prop `之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。 

```
注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。
```

##### 1、保持单向数据流的重要性

在父组件中，**一个变量往往关联着多个变量或动作** 

- 如果在子组件中修改了传进来的prop对象，并且没有在父组件中`watch`该prop对象，不会主动触发其他变量的修改或动作的发生，导致数据局部展示错误。
- 如果主动`watch`该prop对象，由于无法准确预知修改的来源和方式，从而大大增加了父组件的维护难度。

##### 2、组件的分类

- `抽象通用组件` 
  - **基本数据**和**默认值**都是通过prop传入，没有在组件内依赖`ajax`异步数据
  - **保证不修改prop**，**交互结果**通过`v-on`等通知方式返回
- `业务通用组件` 
  - 在组件内依赖业务`ajax`异步数据
  - 在需要进行`增删改查`操作的表格中，很难做到完全不修改prop，`CURD`操作往往依赖于业务逻辑。如果不直接修改，则需要添加N多的`v-on`事件

<!--more-->

<br/>

#### 二、组件通信规范

Vue 组件有多种通信方式，选择合理的通信方式，**尽可能保证数据的单向流动**。

- `props`，遵守使用props来初始化子组件，尽量不修改props值
- `refs`，父组件通过refs获取子组件实例，适用于**主动**获取（只读）子组件数据或调用子组件方法修改数据
- `v-on`，适用于被动获取子组件数据
- `bus`，监听自定义事件，适用于被动获取组件数据，忽略组件层级关系

注：基于**数据劫持**和**发布订阅模式**。为了避免修改，可以直接使用深度拷贝来初始化。

<br/>