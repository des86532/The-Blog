---
title:  vue-內置組件
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## Transition

`:css="false"` 會讓 vue 跳過 css 過度的自動探測，但就必須手動添加動畫效果

```javascript
<Transition
  @before-enter="onBeforeEnter"
  @enter="onEnter"
  @leave="onLeave"
  :css="false"
>
  ...
</Transition>

```

```javascript
<script setup>
import { ref } from 'vue'
import gsap from 'gsap'

const show = ref(true)

function onBeforeEnter(el) {
  gsap.set(el, {
    scaleX: 0.25,
    scaleY: 0.25,
    opacity: 1
  })
}
  
function onEnter(el, done) {
  gsap.to(el, {
    duration: 1,
    scaleX: 1,
    scaleY: 1,
    opacity: 1,
    ease: 'elastic.inOut(2.5, 1)',
    onComplete: done
  })
}

function onLeave(el, done) {
	gsap.to(el, {
    duration: 0.7,
    scaleX: 1,
    scaleY: 1,
    x: 300,
    ease: 'elastic.inOut(2.5, 1)'
  })
  gsap.to(el, {
    duration: 0.2,
    delay: 0.5,
    opacity: 0,
    onComplete: done
  })
}
</script>
```

### 出現時過渡效果

如果你想在某個節點初次渲染時應用一個過渡效果，你可以添加 `appear` prop：

```
<Transition appear>
  ...
</Transition>

```

## Teleport

> 它可以將一個組件內部的一部分模板“傳送”到該組件的 DOM 結構外層的位置去。

### 禁用 Teleport

```html
<Teleport :disabled="isMobile">
  ...
</Teleport>
```

## Suspense

`實驗性功能 !`

> 用來在組件樹中協調對異步依賴的處理。它讓我們可以在組件樹上層等待下層的多個嵌套異步依賴項解析完成，並可以在等待時渲染一個加載狀態。

目前只有兩個 slot: `default` `fallback`
```html
<Suspense>
  <!-- 具有深層異步依賴的組件 -->
  <Dashboard />

  <!-- 在 #fallback 插槽中顯示 “正在加載中” -->
  <template #fallback>
    Loading...
  </template>
</Suspense>
```

Suspense 有三個事件:
- `pending` 事件是在進入掛起狀態時觸發。
- `resolve` 事件是在 `default` 插槽完成獲取新內容時觸發。
- `fallback` 事件則是在 `fallback` 插槽的內容顯示時觸發。