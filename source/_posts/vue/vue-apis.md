---
title:  vue-apis
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## watchEffect

預設 `flush: pre`

## watchPostEffect

> [`watchEffect()`](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 使用 `flush: 'post'` 選項時的別名

偵聽器延遲到組件渲染之後再執行

```javascript
watchEffect(() => {}, {
  flush: 'post',
  onTrack(e) {
    debugger
  },
  onTrigger(e) {
    debugger
  }
})
```

## watchSyncEffect

> [`watchEffect()`](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 使用 `flush: 'sync'` 選項時的別名

響應式依賴發生改變時立即觸發偵聽器

```javascript
watchEffect(() => {}, {
  flush: 'sync',
  onTrack(e) {
    debugger
  },
  onTrigger(e) {
    debugger
  }
})
```

## shallowRef

> ref() 的淺層作用

```javascript
const state = shallowRef({ count: 1 })

// 不會觸發更改
state.value.count = 2

// 會觸發更改
state.value = { count: 2 }
```

## triggerRef

> 強制觸發依賴淺層 ref 的副作用，這主要在對淺引用的內部值進行深度變更後使用

```javascript
const shallow = shallowRef({
  greet: 'Hello, world'
})

// 触发该副作用第一次应该会打印 "Hello, world"
watchEffect(() => {
  console.log(shallow.value.greet)
})

// 这次变更不应触发副作用，因为这个 ref 是浅层的
shallow.value.greet = 'Hello, universe'

// 打印 "Hello, universe"
triggerRef(shallow)
```

## customRef

> 創建一個自定義的 ref，顯式聲明對其依賴追踪和更新觸發的控制方式

防抖函數

```javascript
import { customRef } from 'vue'

export function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger()
        }, delay)
      }
    }
  })
}
```

在組件中使用:

```javascript
<script setup>
import { useDebouncedRef } from './debouncedRef'
const text = useDebouncedRef('hello')
</script>

<template>
  <input v-model="text" />
</template>
```

## shallowReactive

> reactive() 的淺層作用

```javascript
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改狀態自身的屬性是響應式的
state.foo++

// ...但下層嵌套對像不會被轉為響應式
isReactive(state.nested) // false

// 不是響應式的
state.nested.bar++
```

## shallowReadonly

> readonly 的淺層作用

```javascript
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改狀態自身的屬性會失敗
state.foo++

// ...但可以更改下層嵌套對象
isReadonly(state.nested) // false

// 這是可以通過的
state.nested.bar++
```

## toRaw

> 返回原始對象

```javascript
const foo = {}
const reactiveFoo = reactive(foo)

console.log(toRaw(reactiveFoo) === foo) // true
```

## markRaw

> 將一個對象標記為不可轉為代理

## effectScope

> 創建一個 effect 作用域，可以捕獲其中所創建的響應式副作用 (即計算屬性和偵聽器)，這樣捕獲到的副作用可以一起處理

```javascript
const scope = effectScope() // 創建作用域

scope.run(() => {
  const doubled = computed(() => counter.value * 2)

  watch(doubled, () => console.log(doubled.value))

  watchEffect(() => console.log('Count: ', doubled.value))
})

// 处理掉当前作用域内的所有 effect
scope.stop()
```

[揭秘Vueuse是怎么利用effectScope实现React hook的特性的？ - 掘金 (juejin.cn)](https://juejin.cn/post/7059303895880695839)
## getCurrentScope

> 如果有的話，返回當前活躍的 effect 作用域

## onScopeDispose

> 在當前活躍的 effect 作用域上註冊一個處理回調函數。當相關的 effect 作用域停止時會調用這個回調函數。

這個方法可以作為可複用的組合式函數中 onUnmounted 的替代品，它並不與組件耦合，因為每一個 Vue 組件的 setup() 函數也是在一個 effect 作用域中調用的。

```javascript
import { onScopeDispose } from 'vue'

const scope = effectScope()

scope.run(() => {
  onScopeDispose(() => {
    console.log('cleaned!')
  })
})

scope.stop() // logs 'cleaned!'
```

## isRef

> 檢查某個值是否為 ref

## unref

> 如果參數是 ref，則返回內部值，否則返回參數本身。這是 val = isRef(val) ? val.value : val 計算的一個語法糖。

```javascript
function useFoo(x: number | Ref<number>) {
  const unwrapped = unref(x)
  // unwrapped 现在保证为 number 类型
}
```

## toRef

> 基於響應式對像上的一個屬性，創建一個對應的 ref。這樣創建的 ref 與其源屬性保持同步：改變源屬性的值將更新 ref 的值，反之亦然。

解構後並保持響應性

```javascript
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

// 更改该 ref 会更新源属性
fooRef.value++
console.log(state.foo) // 2

// 更改源属性也会更新该 ref
state.foo++
console.log(fooRef.value) // 3

```

## toRefs

> 將一個響應式對象轉換為一個普通對象，這個普通對象的每個屬性都是指向源對象相應屬性的 ref。每個單獨的 ref 都是使用 toRef() 創建的。

將對象上現有的屬性，都改變成 ref 響應式屬性，相當於對每個屬性都做了 `toRef`

```javascript
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)
/*
stateAsRefs 的类型：{
  foo: Ref<number>,
  bar: Ref<number>
}
*/

// 这个 ref 和源属性已经“链接上了”
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3
```

## isProxy

> 檢查一個對像是否是由 reactive()、readonly()、shallowReactive() 或 shallowReadonly() 創建的代理

## isReactive

> 檢查一個對象是否由 reactive() or shallowReactive() 創建

## isReadonly

> 檢查傳入的值是否為只讀對象。只讀對象的屬性可以更改，但他們不能通過傳入的對象直接賦值

通过 [`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly) 和 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理都是只读的，因为他们是没有 `set` 函数的 [`computed()`](https://cn.vuejs.org/api/reactivity-core.html#computed) ref。

`shallowReadonly` 的下層嵌套對象不是**只讀**的

```javascript
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

// ...但可以更改下層嵌套對象
isReadonly(state.nested) // false
```
