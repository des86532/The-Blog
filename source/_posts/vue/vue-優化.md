---
title:  vue-優化
comments: false
date: 2023-04-12
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 異步組件

異步組件使用 `defineAsyncComponent` 來建立，可以避免在未使用該 component 時，就下載該 component 的 js 檔

沒使用 defineAsyncComponent import 的 component，不管有沒有使用，只要 import 就會先產生該 js 檔的 request(多一個不必要的 request)

```javascript
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...从服务器获取组件
    resolve(/* 获取到的组件 */)
  })
})
// ... 像使用其他一般组件一样使用 `AsyncComp`

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
```

```javascript
const AsyncComp = defineAsyncComponent({
  // 加载函数
  loader: () => import('./Foo.vue'),

  // 加载异步组件时使用的组件
  loadingComponent: LoadingComponent,
  // 展示加载组件前的延迟时间，默认为 200ms
  delay: 200,

  // 加载失败后展示的组件
  errorComponent: ErrorComponent,
  // 如果提供了一个 timeout 时间限制，并超时了
  // 也会显示这里配置的报错组件，默认值是：Infinity
  timeout: 3000
})

```

## 更新優化

### Props 穩定性

在 Vue 之中，一個子組件只會在其至少一個 props 改變時才會更新。思考以下例：

```html
	<ListItem
	  v-for="item in list"
	  :id="item.id"
	  :active-id="activeId"
	/>
```

在 `<ListItem>` 組件中，它使用了 `id` 和 `activeId` 兩個 props 來確定它是否是當前活躍的那一項。雖然這是可行的，但問題是每當 `activeId` 更新時，列表中的**每一個** `<ListItem>` 都會跟著更新！

理想情況下，只有活躍狀態發生改變的項才應該更新。我們可以將活躍狀態比對的邏輯移入父組件來實現這一點，然後讓 `<ListItem>` 改為接收一個 `active` prop：
 
```html
	<ListItem
	  v-for="item in list"
	  :id="item.id"
	  :active="item.id === activeId"
	/>
```

現在，對於大多數的組件來說，`activeId` 改變時，它們的 `active` prop 都會保持不變，因此它們無需再更新。總結一下，這個技巧的核心思想就是讓傳給子組件的 props 盡量保持穩定。

### v-once
**僅渲染元素和組件一次，並跳過之後的更新**

在隨後的重新渲染，元素/組件及其所有子項將被當作靜態內容並跳過渲染。這可以用來優化更新時的性能。

### v-memo
緩存一個模板的子樹。在元素和組件上都可以使用。為了實現緩存，該指令需要傳入一個固定長度的依賴值數組進行比較
	
如果數組裡的每個值都與最後一次的渲染相同，那麼整個子樹的更新將被跳過，連 virtual DOM 的創建也會被跳過，舉例來說：

```html
	<div v-memo="[valueA, valueB]">
	  ...
	</div>
```

**v-memo="[]"  與 v-once 效果相同**

#### 與 `v-for` 一起使用
`v-memo` 僅用於性能至上場景中的微小優化，應該很少需要。最常見的情況可能是有助於渲染海量 `v-for` 列表 (長度超過 1000 的情況)：

```html
<div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
  <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
  <p>...more child nodes</p>
</div>
```

當組件的 `selected` 狀態改變，默認會重新創建大量的 vnode，儘管絕大部分都跟之前是一模一樣的。 `v-memo` 用在這裡本質上是在說“只有當該項的被選中狀態改變時才需要更新”。這使得每個選中狀態沒有變的項能完全重用之前的 vnode 並跳過差異比較。注意這裡 memo 依賴數組中並不需要包含 `item.id`，因為 Vue 也會根據 item 的 `:key` 進行判斷。

> 當搭配 `v-for` 使用 `v-memo`，確保兩者都綁定在同一個元素上。 **`v-memo` 不能用在 `v-for` 內部。 **

`v-memo` 也能被用於在一些默認優化失敗的邊際情況下，手動避免子組件出現不需要的更新。但是一樣的，開發者需要負責指定正確的依賴數組以免跳過必要的更新。

## 通用優化

> 以下技巧能同时改善页面加载和更新性能。

### 大型虛擬列表

所有的前端應用中最常見的性能問題就是渲染大型列表。無論一個框架性能有多好，渲染成千上萬個列表項**都會**變得很慢，因為瀏覽器需要處理大量的 DOM 節點。

但是，我們並不需要立刻渲染出全部的列表。在大多數場景中，用戶的屏幕尺寸只會展示這個巨大列表中的一小部分。我們可以通過**列表虛擬化**來提升性能，這項技術使我們只需要渲染用戶視口中能看到的部分。

要實現列表虛擬化並不簡單，你可以直接使用現有的社區庫：

-   [vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)
-   [vue-virtual-scroll-grid](https://github.com/rocwang/vue-virtual-scroll-grid)
-   [vueuc/VVirtualList](https://github.com/07akioni/vueuc)

### 减少大型不可變數據的響應性開銷

Vue 的響應性系統默認是深度的。雖然這讓狀態管理變得更直觀，但在數據量巨大時，深度響應性也會導致不小的性能負擔，因為每個屬性訪問都將觸發代理的依賴追踪。好在這種性能負擔通常這只有在處理超大型數組或層級很深的對象時，例如一次渲染需要訪問 100,000+ 個屬性時，才會變得比較明顯。因此，它只會影響少數特定的場景。

Vue 確實也為此提供了一種解決方案，通過使用 [`shallowRef()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 和 [`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 來繞開深度響應。淺層式 API 創建的狀態只在其頂層是響應式的，對所有深層的對像不會做任何處理。這使得對深層級屬性的訪問變得更快，但代價是，我們現在必須將所有深層級對象視為不可變的，並且只能通過替換整個根狀態來觸發更新：

```javascript
const shallowArray = shallowRef([
  /* 巨大的列表，裡面包含深層的對象 */
])

// 這不會觸發更新...
shallowArray.value.push(newObject)
// 這才會觸發更新
shallowArray.value = [...shallowArray.value, newObject]

// 這不會觸發更新...
shallowArray.value[0].foo = 1
// 這才會觸發更新
shallowArray.value = [
  {
    ...shallowArray.value[0],
    foo: 1
  },
  ...shallowArray.value.slice(1)
]
```

### 避免不必要的组件抽象

有些時候我們會去創建[無渲染組件](https://cn.vuejs.org/guide/components/slots.html#renderless-components)或高階組件 (用來渲染具有額外 props 的其他組件) 來實現更好的抽像或代碼組織。雖然這並沒有什麼問題，但請記住，**組件實例比普通 DOM 節點要昂貴得多**，而且為了邏輯抽象創建太多組件實例將會導致性能損失。

需要提醒的是，只減少幾個組件實例對於性能不會有明顯的改善，所以如果一個用於抽象的組件在應用中只會渲染幾次，就不用操心去優化它了。考慮這種優化的最佳場景還是在大型列表中。想像一下一個有 100 項的列表，每項的組件都包含許多子組件。在這裡去掉一個不必要的組件抽象，可能會減少數百個組件實例的無謂性能消耗。

```html
<MouseTracker v-slot="{ x, y }">
  Mouse is at: {{ x }}, {{ y }}
</MouseTracker>
```

無渲染組件主要用來封裝某些功能或是商業邏輯，可以用hook來取代無渲染組件