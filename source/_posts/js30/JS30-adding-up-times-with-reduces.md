---
title: Adding Up Times With Reduces
comments: false
date: 2019-05-25
categories:
    - JS30
tags:
    - JS30
---

# Adding Up Times with Reduce

## 主題
利用`map()`與`reduce()`來取得播放清單的總秒數。

[Adding Up Times with Reduce](https://des86532.github.io/javascript-30/18_Adding-Up-Times-with-Reduce/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/18_Adding-Up-Times-with-Reduce)

## 步驟

### Step1. 取得全部的時間值

在HTML中，時間資訊放在`<li data-time>`中，
所以透過querySelectorAll來取得，
因為接著會使用map及reduce操作，
資料型態必須先轉為Array。
```
// 透過Array.from或是[...]來將querySelectorAll取回的NodeList轉Array
const timeNodes = Array.from(document.querySelectorAll('[data-time'));
const timeNodes = [...document.querySelectorAll('[data-time]')]
```

### Step2. 將取回的資料轉為秒數並加總
```
const seconds = timeNodes
      // 取出每個元素中的data-time資料
      .map(node => node.dataset.time)
      .map(timeCode => {
        // 用解構賦值的方式分別取出split(':')後的分與秒
        // 再透過一個map執行parseFloat將字串轉數值
        const [mins, secs] = timeCode.split(':').map(parseFloat);
        // 回傳這組資料轉換後的總秒數
        return (mins * 60) + secs;
      })
      // 用reduce來加總每次執行結果
      .reduce((total, seconds) => total + seconds);
```

### Step3. 把總秒數轉為時分秒格式
```
// 利用取得的總秒數來進行總共時分秒的計算
// 使用Math.floor取整數，再利用%來操作餘數
  const hours = Math.floor(seconds/3600)
  const min = Math.floor((seconds % 3600)/60)
  const sec = (seconds % 3600) % 60
```

### Step4. 印出結果
```
console.log(hours,min,sec)
```
## 其他

這篇也算是之前學習的再次運用，
比較特別的是發現map中可以直接使用function！
```
const [mins, secs] = timeCode.split(':').map(parseFloat);
//等同於
const [mins, secs] = timeCode.split(':').map(function(str){
    return parseFloat(str);
});
```