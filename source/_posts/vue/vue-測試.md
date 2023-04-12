---
title:  vue-測試
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 單元測試

> 重視邏輯上的正確性

```javascript
// helpers.js
export function increment (current, max = 10) {
  if (current < max) {
    return current + 1
  }
  return current
}
```

```javascript
// helpers.spec.js
import { increment } from './helpers'

describe('increment', () => {
  test('increments the current number by 1', () => {
    expect(increment(0, 10)).toBe(1)
  })

  test('does not increment the current number over the max', () => {
    expect(increment(10, 10)).toBe(10)
  })

  test('has a default max of 10', () => {
    expect(increment(10)).toBe(10)
  })
})
```

### 組件的單元測試

一個組件可以通過兩種方式測試：

1.  白盒：單元測試
    
    白盒測試知曉一個組件的實現細節和依賴關係。它們更專注於將組件進行更 **獨立** 的測試。這些測試通常會涉及到模擬一些組件的部分子組件，以及設置插件的狀態和依賴性（例如 Vuex）。
    
2.  黑盒：組件測試
    
    黑盒測試不知曉一個組件的實現細節。這些測試盡可能少地模擬，以測試組件在整個系統中的集成情況。它們通常會渲染所有子組件，因而會被認為更像一種“集成測試”。

## 組件測試

組件測試應該捕捉組件中的 prop、事件、提供的插槽、樣式、CSS class 名、生命週期鉤子，和其他相關的問題。

組件測試應該像用戶一樣，通過與組件互動來測試組件和其子組件之間的交互。例如，組件測試應該像用戶那樣點擊一個元素，而不是編程式地與組件進行交互。

> 測試這個組件做了什麼，而不是測試它是怎麼做到的

- **推薦的做法**
	- 對於 **視圖** 的測試：根據輸入 prop 和插槽斷言渲染輸出是否正確。
	- 對於 **交互** 的測試：斷言渲染的更新是否正確或觸發的事件是否正確地響應了用戶輸入事件。

-   **應避免的做法**
    
    - 不要去斷言一個組件實例的私有狀態或測試一個組件的私有方法。測試實現細節會使測試代碼太脆弱，因為當實現發生變化時，它們更有可能失敗並需要更新重寫。
    
    - 組件的最終工作是渲染正確的 DOM 輸出，所以專注於 DOM 輸出的測試提供了足夠的正確性保證（如果你不需要更多其他方面測試的話），同時更加健壯、需要的改動更少。
    
    - 不要完全依賴快照測試。斷言 HTML 字符串並不能完全說明正確性。應當編寫有意圖的測試。
    
    - 如果一個方法需要測試，把它提取到一個獨立的實用函數中，並為它寫一個專門的單元測試。如果它不能被直截了當地抽離出來，那麼對它的調用應該作為交互測試的一部分。

> [测试 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/scaling-up/testing.html#why-test)