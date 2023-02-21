---
title: Sort Without Articles
comments: false
date: 2019-05-24
categories:
    - JS30
tags:
    - JS30
---

# 17 — Sort Without Articles

![sort_without_articles](0_f5N27GRM46o-5Mx7.png)

## 主題

介紹如何將陣列在排除部分文字的情況下排序。

[Sort Without Articles](https://des86532.github.io/javascript-30/17_Sort-Without-Articles/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/17_Sort-Without-Articles)

## 步驟

### Step1. 建立篩選的function

使用replace搭配正規表示式來將包含了a, the, an開頭的文字替換為空白。
```
function strip(bandName) {
    return bandName.replace(/^(a |the |an )/i, '').trim();
}
```
> [MDN-RegExp](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Regular_Expressions)
[RegExp-Test](https://regex101.com)

## Step2. 對目標陣列進行篩選與排序

這裡將原本的寫法與簡寫放在一起，可以發現整體簡潔不少。
```
//原本的寫法
const sortedBands = bands.sort(function(a, b){
    if(strip(a) > strip(b)) {
        return 1;
    }else {
        return -1;
    }
})
//利用箭頭函數與三元運算式的簡寫：
const sortedBands = bands.sort((a, b) => (strip(a) > strip(b)) ? 1 : -1);
```

### Step3. 把排序完的渲染到HTML中

使用map與join來組成`<li>`元素放置
```
document.querySelector('#bands').innerHTML = 
      sortedBands.map(band => `<li>${band}</li>`).join('');
```
使用join(”)修改連結符號為空白, 否則原先陣列的分隔符號是,也會一併渲染在html中。

## 探索

這邊做了下match,includes,與正規表達的比較

### match()
```
string.match (regexp)
```
比對字串與規則運算式，然後傳回包含該搜尋結果的陣列。
結果會以陣列顯示
```
let str =  "azcafAJAC";
let re = /[a-c]/;
let result = str.match(re)
```
輸出結果 ： result = [a]
```
let str = "azcafAJAC";
let re = /[a-c]/g;
let result = str.match(re)
```
輸出結果 ： result = [a,c,a]

### includes
```
arr.includes(searchElement[, fromIndex])
```
傳回布林值，指出字串物件中是否包含傳入的字串。
```
// Returns true 
"abcde".includes("cd")
"abcde".includes("cd", 2)
// Returns false
"abcde".includes("CD")
"abcde".includes("cd", 3)
```