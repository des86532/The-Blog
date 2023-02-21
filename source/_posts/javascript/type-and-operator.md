---
title: 克服 JS 的奇怪部分 — 型別與運算子
comments: false
date: 2019-06-21
categories:
    - Javascript
tags:
    - javascript
---

# Javascript 處理型別的方式：動態型別

你不需要在設定變數時告訴 Javascript 你的變數是甚麼型別，他會在運行程式時自動知道

## PRIMITIVE TYPE(純值)

a type of data that represents a single value, not an object

- undefined
javascript 所有變數的初始值，表示尚未存在
不應該設定任何一個變數等於 undefined

- null
也表示不存在，但比較適合來表示一個東西不存在，變數沒有值

- Boolean
true or false

- Number
他是浮點數

- String
雙引號以及單引號都可以表示字串

- symbol
之後的章節會說明

## 運算子
運算子都是函數

### OPERATOR PRECEDDENCE(運算子的優先性)
具有高優先性的運算子會先計算

### ASSOCIATIVITY(相依性)
代表運算子被計算的順序

左到右的相依性，稱作 “左相依性”
右到左的相依性，稱作 “右相依性”

當兩個運算子有相同的優先性，就由相依性判斷
```
var a = 2, b = 3, c = 4;
a = b = c;
console.log(a); // 4
console.log(b); // 4
console.log(b); // 4
```
= 為右相依性

> [Operator-Precedence-In-Javascript](Operator-Precedence-In-Javascript.pdf)

### COERCION(強制型轉)
轉換一個值的型別
```
var a = 1 + '2';
console.log(a) // 12 (string)
```

Javascript 會自動將數字 1 轉為字串 1

### 比較運算子
```
console.log(3 < 2 < 1);
console.log(false < 1);
console.log(0 < 1); // true
console.log(1 < 2 < 3);
console.log(true < 3);
console.log(1 < 3); // true
```

false 被強制型轉為 0
true 被強制型轉為 1
== 在比較時接受強制型轉
=== 不接收強制型轉(完全相同)

## 備註
```
Number(undefined) // NaN
Number(null) // 0
false == 0 // true
null == 0 // false
null < 1 // true
"" == 0 // true
"" == false // true
```
null 在比較時不會型轉為 Number

> [Equalty-Comparison-And-Sameness](Equalty-Comparison-And-Sameness.pdf)

## EXEISTENCE(存在) & BOOLEAN(布林)
```
var a;
// undefined 被強制型轉為 false
if(a) {  
    console.log('something is there')
}
```

### 數字 0
0 並不是不存在，但強制型轉會轉為 false
```
var a = 0;
// 0 被強制型轉為 false
if(a) {
    console.log('something is there')
}
// 建議改用
if(a || a === 0) {
    console.log('something is there')
}
```

## 預設值
```
function greet(name) {
    console.log(name)             // undefined
    console.log('Hello ' + name); // Hello undefined
}
greet();
function greet(name = 'Rick') {
    console.log(name)             // Rick
    console.log('Hello ' + name); // Hello Rick
}
greet();
```
