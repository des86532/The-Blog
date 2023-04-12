---
title:  vue-事件修飾符
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

-   `.stop`
-   `.prevent`
-   `.self`

.stop: 阻止任何事件
.prevent: 阻止預設原生事件
.self: 限制事件的觸發範圍，點在其子元素的話並不會觸發

```html
<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>
```

```html
<template>
  <div @click.self="handleClick">
    <button>Click me</button>
  </div>
</template>
```

當單擊`<div>`元素時，事件處理程序將被觸發，
但是如果單擊了`<button>`元素，則事件處理程序將不會被觸發，因為它不在`.self`修飾符的範圍內

- `.capture`
- `.once`
- `.passive`

> 先捕獲，再冒泡

.capture : 從外觸發到內，一般的觸發事件會忽略捕獲階段，從冒泡階段觸發
.once: 只會觸發一次
.passtive: 避免預設的原生事件被阻止，所以不能使用 `event.preventDefault`，會報錯

```html
<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>

<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>

```

使用修飾符時需要注意調用順序，因為相關程式碼是以相同的順序生成的。因此使用`@click.prevent.self` 會阻止 **元素及其子元素的˙所有點擊事件的默認行為**，而`@click.self.prevent`則只會阻止**對元素本身的點擊事件的默認行為**。