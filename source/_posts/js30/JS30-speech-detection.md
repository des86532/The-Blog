---
title: Speech Detection
comments: false
date: 2019-05-27
categories:
    - JS30
tags:
    - JS30
---

# 20 — Speech Detection

## 主題

利用`SpeechRecognition`來做語音識別，
並透過`interimResults`來輸出識別的結果。

[Speech Detection](https://des86532.github.io/javascript-30/20_Speech-Detection/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/20_Speech-Detection)

## 步驟
### Step1. 啟動Local Server

這個練習需要使用到 local server，
如果你已經有一個可在本機 run 起來的 server 可以直接使用，
或在這層資料夾底下運行`npm install`來安裝`browser-sync`，
安裝完成後可以透過指令 npm start 來啟動 localserver(預設port3000)，
npm指令需要下載node.js來使用

### Step2. 將SpeechRecognition建立起來
```
// 將全域環境中的SpeechRecognition指好(依據不同瀏覽器)
window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
// 建立一個變數recognition來放為語音識別功能
const recognition = new SpeechRecognition();
// 讓語音識別回傳識別後的資訊（預設為false)
recognition.interimResults = true;
```
> [MDN-SpeechRecognition](https://developer.mozilla.org/zh-TW/docs/Web/API/SpeechRecognition)

### Step3. 把輸出區域準備好
```
// 建立一個p元素在html設定好的文字區中
let p = document.createElement('p');
const words = document.querySelector('.words');
words.appendChild(p);
```

### Step4. 對識別系統做監聽
識別回傳的資料是`NodeList`，所以要用`map`操作得先轉`array`
```
// 監聽識別回傳
recognition.addEventListener('result', e => {
  // 將回傳資料先轉為array來操作
  const transcript = Array.from(e.results)
    // 透過map取得回傳陣列中的第0筆
    .map(result => result[0])
    // 在取得第0筆中的transcript
    .map(result => result.transcript)
    // 用join把連結符號消掉
    .join('')
  // 把回傳內容塞到p元素中
  p.textContent = transcript;
  // 如果回傳內容已經結束（一段話的結尾）在建立一個新的p元素來放下一段文字
  if (e.results[0].isFinal) {
    p = document.createElement('p');
    words.appendChild(p);
  }
})
// 監聽如果語音識別結束，則在開啟一次新的識別
recognition.addEventListener('end', recognition.start);
// 開始識別
recognition.start();
```

## JavaScript語法&備註

### appendChild

加入一個新的標籤
```
let node  = document.createElement('p')
let textnode = document.createTextNode('Water')
node.appendChild(textnode)     //此時node已改變
document.getElementById('mylist').appendChild(node)
//此時會創立一個新的<p>Water</p> 在class="mylist" 裡面
let node = document.getElementById('mylist1').lastChild
document.getElementById('mylist2').appendChild(node)
//class="mylist1"中的最後一個元素會被移動到class="mylist2"中
```

> [MDN-appendChild](https://www.w3schools.com/jsref/met_node_appendchild.asp)
[MDN-createTextNode](https://www.w3schools.com/jsref/met_document_createtextnode.asp)