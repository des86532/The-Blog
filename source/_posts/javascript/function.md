---
title: 克服 JS 奇怪的地方 — 函數程式設計
comments: false
date: 2019-06-25
categories:
    - Javascript
tags:
    - javascript
---

## 建立一個 function，可以帶入其他 function 改變 array
```
function mapForEach(arr, fn) {
    
    var newArr = [];
    for (var i=0; i < arr.length; i++) {
        newArr.push(
            fn(arr[i])   
        )
    };
    
    return newArr;
}
```

## 利用mapForEach function 回傳 array 兩倍大
```
var arr1 = [1,2,3];
var arr2 = mapForEach(arr1, function(item) {
   return item * 2; 
});
console.log(arr2);  // [2,4,6]
```

## 建立一個 function 檢驗 array 內的值
```
var checkPastLimit = function(limiter, item) {
    return item > limiter;   
}
var arr4 = mapForEach(arr1, checkPastLimit.bind(this,1));
console.log(arr4);   // [false, true, true]
```

## 將 function 重構，checkPastLimit 精簡化只需要帶入一個值
```
var checkPastLimitSimple = function(limiter) {
    return function(limiter, item) {
        return item > limiter
    }.bind(this,limiter)
}
var arr5 = mapForEach(arr1, checkPastLimitSimple(1));
console.log(arr5);  // [false, true, true]
```