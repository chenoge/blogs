---
title: Vue-作用域插槽
date: 2018-04-17 20:26:19
tags: [vue,插槽]
---

作用域插槽是一种特殊类型的插槽，用作一个 (能被传递数据的) 可重用模板，来代替已经渲染好的元素。

在子组件中，只需将数据传递到插槽，就像你将 prop 传递数据给子组件一样，传递到父组件：

```html
<div class="child">
    <slot text="hello from child"></slot>
</div>
```

在父级中，具有特殊特性 slot-scope 的 <template> 元素必须存在，表示它是作用域插槽的模板。slot-scope 的值将被用作一个临时变量名，此变量接收从子组件传递过来的 prop 对象： 

```html
<div class="parent">
    <child>
        <template slot-scope="props">
            <span>hello from parent</span>
            <span>{{ props.text }}</span>
        </template>
    </child>
</div>
```

如果我们渲染上述模板，得到的输出会是： 

```html
<div class="parent">
    <div class="child">
        <span>hello from parent</span>
        <span>hello from child</span>
    </div>
</div>
```

