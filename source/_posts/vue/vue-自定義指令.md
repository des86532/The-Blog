---
title:  vue-自定義指令
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 使用方式

### 註冊在component:

#### 組合式:
```html
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```

#### 選項式:
```javascript
export default {
  setup() {
    /*...*/
  },
  directives: {
    // 在模板中启用 v-focus
    focus: {
      /* ... */
    }
  }
}
```

### 註冊在應用層級:
```javascript
const app = createApp({})

// 使 v-focus 在所有组件中都可用
app.directive('focus', {
  /* ... */
})
```

## 指令鉤子(Directive Hooks)

```javascript
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}

```

## 鉤子參數
-   `el`：指令绑定到的元素。这可以用于直接操作 DOM。
    
-   `binding`：一个对象，包含以下属性。
    
    -   `value`：传递给指令的值。例如在 `v-my-directive="1 + 1"` 中，值是 `2`。
    -   `oldValue`：之前的值，仅在 `beforeUpdate` 和 `updated` 中可用。无论值是否更改，它都可用。
    -   `arg`：传递给指令的参数 (如果有的话)。例如在 `v-my-directive:foo` 中，参数是 `"foo"`。
    -   `modifiers`：一个包含修饰符的对象 (如果有的话)。例如在 `v-my-directive.foo.bar` 中，修饰符对象是 `{ foo: true, bar: true }`。
    -   `instance`：使用该指令的组件实例。
    -   `dir`：指令的定义对象。
-   `vnode`：代表绑定元素的底层 VNode。
    
-   `prevNode`：之前的渲染中代表指令所绑定元素的 VNode。仅在 `beforeUpdate` 和 `updated` 钩子中可用。

### 舉例

```html
<div v-example:foo.bar="baz">
```

#### binding:
```javascript
{
  arg: 'foo',
  modifiers: { bar: true },
  value: /* `baz` 的值 */,
  oldValue: /* 上一次更新时 `baz` 的值 */
}
```


添加指令在組件上時，需要注意的是組件可能含有多個根節點。當應用到一個多根組件時，指令將會被忽略且拋出一個警告。和 attribute 不同，指令不能通過 `v-bind="$attrs"` 來傳遞給一個不同的元素。總的來說，**不**推薦在組件上使用自定義指令。