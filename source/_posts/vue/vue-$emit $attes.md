---
title:  vue- $emit $attes
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## emit

同下列方法 `$emit` 可以直接用在 `template` 中，`$emit('submit', { email, password })`

```html
<script setup>
const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
</script>

```

和對 props 添加類型校驗的方式類似，所有觸發的事件也可以使用對象形式來描述。

要為事件添加校驗，那麼事件可以被賦值為一個函數，接受的參數就是拋出事件時傳入 `emit` 的內容，返回一個布爾值來表明事件是否合法。

## attrs

> 如果組件是以單個元素渲染，父組件的 attribute 會自動添加到子組件的根元素上
> 当应用到一个多根组件时，會發出警告，因為 vue 沒辦法知道 父組件的 attribute 要添加到哪

`$attrs` 可以直接用在 `template` 中

取得 `attrs`
```html
<script setup>
import { useAttrs } from 'vue'

const attrs = useAttrs()
</script>
```

### 禁用 Attributes 繼承

```html
<script>
// 使用普通的 <script> 来声明选项
export default {
  inheritAttrs: false
}
</script>

<script setup>
// ...setup 部分逻辑
</script>
```

如果想要部分不完全禁止 `Attributes` 繼承，可以用下列方式

```html
<div class="btn-wrapper">
  <button class="btn" v-bind="$attrs">click me</button>
</div>

<script>
// 使用普通的 <script> 来声明选项
export default {
  inheritAttrs: false
}
</script>
```