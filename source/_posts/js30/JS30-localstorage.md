---
title: LocalStorage
comments: false
date: 2019-05-22
categories:
    - JS30
tags:
    - JS30
---

# 15 — LocalStorage

![localstorage-demoImage](0_F4AHYzuFvW-PI5YJ.png)

## 主題

這篇介紹LocalStorage的用法，
透過一個小菜單來透過localstorage做資料增刪功能。

[LocalStorage](https://des86532.github.io/javascript-30/15_LocalStorage/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/15_LocalStorage)

## 步驟

### Step1. 基礎設定

作者已經設定好這篇練習用的html與css，
主要的架構由一個div包著ul與from，
類似Todo-List的清單(ul)與輸入欄位(form)。

### Step2. 撰寫輸入欄位新增功能

首先取得 form 元素及 ul，並宣告一個空陣列來存放新增資料。
```
const addItems = document.querySelector('.add-items');
const itemsList = document.querySelector('.plates');
const items = [];
```
接著撰寫一個`addItem`，參照備註:
```
function addItem(e) {
// 加上preventDefault()避免每次submit都會重整網頁
e.preventDefault();
// 利用再次querySelector來選取form中的input欄位值
const text = this.querySelector('[name=item]').value;
// 宣告新增要存入的物件，是輸入的文字與是否勾選的狀態(done)
const item = {
    text,
    done: false
}
console.log(item);
// 清空輸入欄位
this.reset();
}
// 監聽submit按鈕
addItems.addEventListener('submit', addItem);
```
這樣每次submit後`items`就會新增在輸入欄位中的物件了！

### Step3. 顯示新增的清單

在上一個步驟中所做的只有存於宣告的陣列中，
並沒有抓出來顯示在HTML中，所以要寫一個function來顯示：
```
// ES6可在function中的參數直接設定參數預設值
function populateList(plates = [], platesList) {
// 使用map搭配join來組成字串，並顯示在html的清單ul中
platesList.innerHTML = plates.map((plate, i) => {
    return `
    <li>
        <input type="checkbox" data-index=${i} id="item${i}" ${plate.done ? 'checked' : ''}/>
        <label for="item${i}">${plate.text}</label>
    </li>
    `;
}).join('');
}
```

然後要記得回到`addItem`中把`platesList`放在`items.push(item)`後面，
讓每次輸入送出後都會執行這個function重新列出組成的物件字串。

### Step4. 加入LocalStorage

當完成了新增功能後，就要進入主軸`LocalStorage`了，
這可以讓瀏覽器存取你設定在這個頁面的資訊，
所以首先在`addItem`中修改加入這段：
```
function addItem(e) {
    populateList(items, itemsList);
    //在localStorage存入item，以key->value的方式，這邊的話第一個參數就是key，第二個參數就是value，value的部分不能直接存入array，所以轉為JSON的方式存入
    localStorage.setItem('items', JSON.stringify(items));
    this.reset();
}
```
這裡將items的資訊存在localStorage中一個叫做`items`的自訂物件中，
注意的是存入的物件或陣列必須透過`JSON.stringify`轉為字串，
因為localStorage中的值是string，否則直接存只會得到”object object”的字串。
接著修改最一開始宣告的`items`:
```
const items = JSON.parse(localStorage.getItem('items')) || [];
```
讓頁面在重整後，先判斷localStorage中是否有存放`items`物件，沒有的話則給空陣列。

### Step5. 儲存checkbox狀態

這裡要新增一個function toggleDone並監聽`itemsList`的click動作，
```
function toggleDone(e) {
// 偵測進來的點擊是input(checkbox)才動作
if (!e.target.matches('input')) return;
// 取得checkbox的data-index值
const el = e.target;
const index = el.dataset.index;
// 利用！來使done的狀態在true/false間切換
items[index].done = !items[index].done;
// 將更新後的狀態寫入localStorage中
localStorage.setItem('items', JSON.stringify(items));
// 更新列表
populateList(items, itemsList);
}
// 監聽click
itemsList.addEventListener('click', toggleDone);
```
## JavaScript語法&備註

### Array.prototype.map()

map方法會建立一個新的陣列，其內容為原陣列的每一個元素經由回呼函式運算後所回傳的結果之集合，
map裡面可以放入function來對前面的array進行處理

> [map-MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### LocalStorage

> [localstorage-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage/LocalStorage)