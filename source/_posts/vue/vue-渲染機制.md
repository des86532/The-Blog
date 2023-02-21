---
title:  vue-渲染機制
comments: false
date: 2023-02-21
categories:
    - Vue
tags:
    - vue
---

## Vue2 缺點

- `Mixin` 容易衝突
- 不支持 `typescript`
- 響應式是透過 `Object.defineProperty` 的 `getter & setter` 實作

> Vue2 現在已經不再維護

## Vue3 優點

- 打包大小減少 41%
- 初次渲染快 55%，更新快 133%
- 響應式是透過 `Proxy`，像是資料的攔截器
- 考慮到 tree-shaking，故所有 api 只能獨立 import，不預先載入
 
## 生命週期

|  Vue2 |  Vue3 |
| ----- | ------|
| beforeCreate | setup()|
| created | setup()|
| beforeMount | onBeforeMount |
| mounted | onMounted |
| beforeUpdate | onBeforeUpdate |
| updated | onUpdated |
| beforeDestroy | onBeforeUnmount |
| destroyed | onUnmounted |
| errorCaptured | onErrorCaptured |

## 渲染機制

編譯(Compile) -> 掛載(Mount) -> 更新(Patch)
1. 編譯:  Vue templates 編譯成 `render functions(return virtual DOM tree)`
2. 掛載:  渲染器執行 render functions，產生 `virtual DOM tree`，再根據 `virtual DOM tree` 產生 `actual DOM nodes`，這個步驟會追蹤所有響應式依賴，響應式依賴有改變時就會觸發此步驟，稱為 `reactive effect`
3. 更新:  依賴發生改變時(觸發`reactive effect`)，會重新產生新的 `virtual DOM tree`，並與舊的 `virtual DOM tree` 比對，將需要更新的部分更新到 `actual DOM`

![[assets/render-pipeline.03805016.png]]
### 虛擬 DOM

vnode 組成虛擬 DOM 樹，vnode 如下:
```js
const vnode = {
	type: 'div',
	props: {
		id: 'hello'
	},
	children: [
		/* 更多 vnode */
	]
}
```

## 模板 vs 渲染函數

> 推薦使用模板

模板可以用在大多數場景，且模板因為有固定的寫法，也方便提升效能(有標記)
渲染函數大多只用在需要處理高度動態渲染邏輯的可重用組件中

編譯器可以靜態分析模板並在生成的代碼中留下 [標記](https://github.com/vuejs/core/blob/main/packages/shared/src/patchFlags.ts)，使運行 `更新` 時盡可能地走捷徑

標記可以告訴渲染器該進行什麼更新，每個標記對應的更新類型不同(class、id...)，
比對標記的方式是透過  [Bitwise operation - Wikipedia] (https://en.wikipedia.org/wiki/Bitwise_operation) 來做比對

### render function

```html
<div>

  <div class="foo">foo</div>

  <div class="foo">foo</div>

  <div class="foo">foo</div>

  <div class="foo">foo</div>

  <div class="foo">foo</div>

  <div>{{ dynamic }}</div>

</div>
```

```javascript
import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, createStaticVNode as _createStaticVNode, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

  

const _hoisted_1 = /*#__PURE__*/_createStaticVNode("<div class=\"foo\">foo</div><div class=\"foo\">foo</div><div class=\"foo\">foo</div><div class=\"foo\">foo</div><div class=\"foo\">foo</div>", 5)

  

export function render(_ctx, _cache, $props, $setup, $data, $options) {

  return (_openBlock(), _createElementBlock("div", null, [

    _hoisted_1,

    _createElementVNode("div", null, _toDisplayString(_ctx.dynamic), 1 /* TEXT */)

  ]))

}

// Check the console for the AST
```

在編譯成 render function 時，`foo` 會被編譯在渲染函數以外，因為是靜態並不需要重複渲染的，
當有夠多的連續靜態dom，這些還會被整合成 `靜態node` ，`靜態node` 會透過 `innerHTML` 來插入 `html`，如果有其他地方需要用到相同的 `靜態node`，則會透過 `cloneNode()` 來複製一個新的DOM

#### createElementBlock

`createElementBlock` : 這個function會用來產生虛擬DOM，以一個Array的方式產生，Array內只有包含動態的node，將tree打平(flat)，這樣在比對虛擬DOM時，只需要一個for loop，這稱為`樹結構打平`

`createElementBlock` 前:

```html
<div> <!-- root block -->
	<div>...</div> <!-- 不会追踪 -->
	<div :id="id"></div> <!-- 要追踪 -->
	<div> <!-- 不会追踪 -->
		<div>{{ bar }}</div> <!-- 要追踪 -->
	</div>
</div>
```

`createElementBlock` 後:

```
div (block root)
	div 带有 :id 绑定
	div 带有 {{ bar }} 绑定
```

### 全局函數訪問

![[assets/全局訪問函數示例.png]]

可在模板中直接被訪問的全局函數
1. Infinity
2. undefined
3. NaN
4. isFinite
5. isNaN
6. parseFloat
7. parseInt
8. decodeURI
9. decodeURIComponent
10. encodeURI
11. encodeURIComponent
12. Math
13. Number
14. Date
15. Array
16. Object
17. Boolean
18. String
19. RegExp
20. Map
21. Set
22. JSON
23. Intl
24. BigInt

>其他的可以透過 `app.config.globalProperties 設定，可以在模板中全局訪問

#vue