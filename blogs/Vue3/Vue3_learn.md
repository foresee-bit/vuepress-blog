---
title: Vue框架
date: 2022-08-29
tags:
 - Vue
categories:
 -  Vue3
---

## VUE使用

### v-if和v-show的区别

`v-if`: v-if对于不符合条件判断的，节点不会生成

v-show:对于v-show不符合判断的，节点会生成，但是不会显示，即display: none

```
二者如何选择？ 如果是一次性使用的用v-if,如果需要频繁切换的话使用v-show
v-for和v-if不能一起使用
```

### vue组件使用

#### props和$emit

在组件的模板表达式中，可以直接使用$emit方法触发自定义事件。

```js
<!-- MyComponent -->
<button @click="$emit(someEvent)">click me</button>
<!-- 父组件可以通过@来监听子组件中的事件 -->
<MyComponent @some-event = "callback" />
```

#### 组件间通讯 - 自定义事件

event.$emit()

#### 组件生命周期

创建(beforeCreate,created)，挂载(beforeMount, mounted)，更新(beforeUpdate,updated)，销毁(beforeDestory,destoryed)。

生命周期（父子组件）

创建时，首先创建(created)父组件，再创建子组件，渲染时，先渲染(mounted)子组件，再渲染父组件。

更新时也是一致的。

### vue高级特性

```
自定义v-model
$nextTick 异步渲染，待DOM渲染完再回调，页面渲染时会将data的修改做整合,多次data修改只会渲染一次
slot
refs
动态、异步组件
keep-alive
mixin
```

#### 如何实现自定义v-model



