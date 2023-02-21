---
title: 克服 JS 奇怪的地方 — bind call apply
comments: false
date: 2019-06-24
categories:
    - Javascript
tags:
    - javascript
---

## bind

複製一個新函數，不會呼叫

第一個參數：設定 this 指向的物件
第二個參數: 會帶入複製 function 的第一個參數(當作固定值，不能被改變的預設參數)
```
var person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
}
var logName = function(lang1, lang2) {
    console.log('Logged: ' + this.getFullName());
    console.log('Arguments: ' + lang1 + ' ' + lang2)
}
var logPersonName = logName.bind(person);
logName(); // this.getFullName is not a function
logPersonName(); 
// Logged: John Doe
// Arguemnts: undefined undefined
logPersonName('en');
// Logged: John Doe
// Arguemnts: en undefined
```

## call

會直接呼叫 function

第一個參數：設定this指向的物件
第二個參數：會帶入function的第一個參數

## apply

會直接呼叫 function，只接受陣列作為參數

call & apply 差別只有在後面的參數， apply 要用陣列
```
var person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
}
var logName = function(lang1, lang2) {
    console.log('Logged: ' + this.getFullName());
    console.log('Arguments: ' + lang1 + ' ' + lang2)
}
logName.call(person, 'en', 'es')
// Logged: John Doe
// Arguemnts: en es
logName.apply(person, ['en', 'es']);
// Logged: John Doe
// Arguemnts: en es
```

## function borrowing
```
var person2 = {
    firstname: 'Jane',
    lastname: 'Doe'
}
console.log(person.getFullName.call(person2)) // Jane Doe
console.log(person.getFullName.apply(person2)) // Jane Doe
// 後面沒有預設參數的時候，call & apply 是一樣的
```

## function currying
```
function multiply(a,b) {
    return a*b;
}
function multipleByTwo(b) {
    var a = 2;
    return a*b
}
var multipleByTwo = multiply.bind(this,2)
// 如果這樣設定，multipleByTwo 這個 function的 a 就永遠會是 2，與上面的 function 意思是一樣的
console.log(multipleByTwo(3))  // 6
console.log(multipleByTwo(9))  // 18
var multipleByTwo = multiply.bind(this,2,3)
// 這樣設定的話，a = 2 b = 3 永遠不變
console.log(multipleByTwo(3))    // 6
console.log(multipleByTwo(9))    // 6
console.log(multipleByTwo(100))  // 6
```