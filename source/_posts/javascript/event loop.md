---
title:  未命名
comments: false
date: 2023-04-12
categories:
    - Javascript
tags:
    - javascript
---

> event loop（事件循環）是一種機制，用於協調 JavaScript Engine Thread 和 Main thread 的運行

## 概述

瀏覽器通常由多個線程組成
1.  主線程（Main Thread）：這是瀏覽器的主線程，負責處理用戶界面和瀏覽器的主要功能，如網址輸入、頁面渲染和 JavaScript 執行等。
    
2.  渲染線程（Rendering Thread）：負責將 HTML、CSS 和 JavaScript 轉換為可見的網頁，然後顯示給用戶。
    
3.  JavaScript 引擎線程（JavaScript Engine Thread）：負責解析和執行 JavaScript 代碼，並將其傳遞給渲染線程。
    
4.  GPU 線程（GPU Thread）：負責處理網頁中的圖形和動畫，並將其渲染到用戶界面上。
    
5.  網路線程（Network Thread）：負責處理網路請求和響應，從網絡中獲取資源，例如 HTML、CSS、JavaScript 和圖像等。
    
6.  儲存線程（Storage Thread）：負責管理瀏覽器的本地儲存，例如 Cookies 和 Local Storage。
    
7.  插件線程（Plugin Thread）：負責執行瀏覽器插件，例如 Flash 和 Java。

**JavaScript Event Loop** 是一種機制，用於協調 `JavaScript Engine Thread` 和 `Main thread` 的運行。JavaScript 是一種單線程語言，這意味著只有一個線程可以執行 JavaScript 代碼，而這個線程就是 JavaScript Engine Thread。當瀏覽器解析 HTML 頁面時，如果遇到 `<script>` 標籤，就會啟動 JavaScript Engine Thread，解析 JavaScript 代碼並執行它。

當 JavaScript Engine Thread 遇到需要等待的操作（例如 I/O 操作或定時器操作）時，它會將這些操作添加到一個任務佇列（Task Queue）中。JavaScript 代碼的執行可能會影響到主執行緒的運行，例如當 JavaScript 代碼需要修改網頁的 DOM 結構時，就必須通過主執行緒來實現。因此，JavaScript Engine Thread 和 Main thread 之間需要有一種機制來協調它們的運行，這就是 JavaScript Event Loop 的作用。

在同一時間，Main thread 也在運行，處理用戶界面的操作和其他任務。當 Main thread 空閒時，它會檢查任務佇列中是否有任務需要執行。如果有，它會從任務佇列中取出一個任務，執行它，然後將控制權返回給 JavaScript Engine Thread。這種方式使 JavaScript 代碼和主執行緒之間可以協調運行，保證瀏覽器的正常運行。

## 介紹

- Heap (堆)
- Call Stack (呼叫堆疊)
- Web api (網路 API)
- Event Queue (事件隊列)
	- Microtask Queue (微任務)
	- Macrotask Queue (宏任務)

### Heap (堆)
是用於動態分配內存的區域，存儲複雜對象和數組等數據結構。當我們創建一個新的對象或數組時，JavaScript 引擎會在 heap 上分配一塊內存，然後將數據存儲在這塊內存中。由於 heap 的大小是動態的，因此我們可以隨時創建和刪除對象，而不需要關心內存的管理。

### Call Stack (呼叫堆疊)
是一種 LIFO（Last-In-First-Out，後進先出）數據結構，用於記錄 JavaScript 函數的執行上下文。每當一個函數被調用時，它的上下文就會被推入 call stack 中，當函數執行完成時，它的上下文就會從 call stack 中彈出。這種方式保證了 JavaScript 代碼的執行順序，讓我們能夠正確地跟踪代碼的執行。

### Web api (網路 API)
提供了一些用於處理瀏覽器事件的 API，例如 DOM 操作、定時器、HTTP 請求等。當瀏覽器事件發生時，Web APIs 會將事件加入事件隊列中等待處理。
[Web APIs | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/API)

### Event Queue (事件隊列)
用於存儲所有需要處理的事件，事件從 Web APIs 中加入事件隊列，然後被送往呼叫堆疊進行處理。

如果呼叫堆疊當前為空，事件隊列中的下一個事件將被處理，事件隊列中`微任務`會優先於`宏任務`被處理，每個宏任務處理完後，會檢查有沒有微任務需要執行，有的話先執行微任務

#### Microtask Queue (微任務)
-   Promise的回調函數
-   MutationObserver的回調函數
-   process.nextTick (Node.js環境)

#### Macrotask Queue (宏任務)
-   I/O操作（例如從網絡上讀取數據）
-   定時器操作（例如setTimeout，setInterval）
-   用户事件（例如點擊，鼠標移動）
-   渲染事件（例如DOM更新）
-   自定義事件

## 範例

```javascript
console.log('1');

// setTimeout2
setTimeout(function() {
  console.log('2');
  // promise3
  Promise.resolve().then(function() {
    console.log('3');
  });
}, 0);

// promise4
Promise.resolve().then(function() {
  console.log('4');
  // setTimeout5
  setTimeout(function() {
    console.log('5');
  }, 0);
});

console.log('6');


// 1 6 4 2 3 5
```

### 第一次事件循環

1. `console.log('1')` 輸出
2. `setTimeout2` 交給 `Web APIs` 處理，0秒後放到 `event queue macrotask queue` 中
3. `promise4` 後的 callback 放到 `event queue microtask queue` 中
4. `console.log('6')` 輸出

microtask queue: promise4
macrotask queue: setTimeout2

共輸出了 `1 6`

### 第二次事件循環

Main Thread 為空，所以處理 event queue，先處理 `微任務` 

1. `console.log('4')` 輸出
2. `setTimeout5` 交給 `Web APIs` 處理，0秒後放到 `event queue macrotask queue` 中

microtask queue: 空
macrotask queue: setTimeout2 setTimeout5

`微任務`為空，換處理`宏任務`

1. `console.log('2')` 輸出，`promise3` 放到 `event queue microtask queue`

microtask queue: promise3
macrotask queue: setTimeout5

`宏任務`處理完了，先看看有沒有`微任務`，有就先執行

1. `console.log('3')` 輸出

microtask queue: 空
macrotask queue: setTimeout5

`微任務`為空，換處理`宏任務`

1. `console.log('5')` 輸出

共輸出 `4 2 3 5`