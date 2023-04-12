---
title:  vue-錯誤處理
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 應用級錯誤處理 :

> 可以用來向追踪服務報告錯誤

### errorHandler:

它可以從下面這些來源中捕獲錯誤：

-   組件渲染器
-   事件處理器
-   生命週期鉤子
-   `setup()` 函數
-   偵聽器
-   自定義指令鉤子
-   過渡 (Transition) 鉤子

```javascript
import { createApp } from 'vue'
const app = createApp(...)
app.config.errorHandler = (err, instance, info) => {
  // 向追踪服務報告錯誤
}
```

### warnHandler:

**警告僅會在開發階段顯示，因此在生產環境中，這條配置將被忽略**

```javascript
app.config.warnHandler = (msg, instance, trace) => {
  // `trace` is the component hierarchy trace
}
```


## 組件調適鉤子

```javascript
<script setup>
import { onRenderTracked, onRenderTriggered } from 'vue'

onRenderTracked((event) => {
  debugger
})

onRenderTriggered((event) => {
  debugger
})
</script>
```