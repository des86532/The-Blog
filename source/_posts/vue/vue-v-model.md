---
title:  vue-v-model
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 範例

父組件

```html
<MyComponent v-model:title="title" />
```

子組件

```html
<!-- MyComponent.vue -->
<script setup>
defineProps(['title'])
defineEmits(['update:title'])
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```

一般的 `v-model` 其實等於 `v-model:modelValue` ，下列的方法也是可行的

```html
<MyComponent v-model="title" />
```

```html
<!-- MyComponent.vue -->
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

## 修飾符

自定義修飾符

```html
<MyComponent v-model.capitalize="myText" />
```

```html
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) } // 設定默認值
})

defineEmits(['update:modelValue'])

console.log(props.modelModifiers) // { capitalize: true }
</script>

<template>
  <input
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

修飾符使用範例:

```html
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

const emit = defineEmits(['update:modelValue'])

function emitValue(e) {
  let value = e.target.value
  if (props.modelModifiers.capitalize) {
    value = value.charAt(0).toUpperCase() + value.slice(1)
  }
  emit('update:modelValue', value)
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>

```