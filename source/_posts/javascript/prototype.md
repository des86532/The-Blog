---
title: 克服 JS 奇怪的地方 — 原型
comments: false
date: 2019-06-26
categories:
    - Javascript
tags:
    - javascript
---

## inheritance (繼承)

繼承的意思其實不用想得太複雜，簡單來說就是指一個物件可以提取到其他物件中的屬性（property）或方法（method）。
## prototype chain (原型鍊)

![prototype](0_jeJNJq2_pmPoM1YK.png)

```
var person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function() {
        return this.firstname + ' ' + this.lastname;
    }
}
var john = {
    firstname: 'John',
    lastname: 'Doe'
}
// don't do this EVER! for demo purposes only !!!!!!
john.__proto__ = person;  
console.log(john.getFullName());  // John Doe
console.log(john.firstname);  // John
```

一個物件裡面除了所給予的屬性值外，另外也包含原型 prototype。

找不到時就會往上層找，obj -> proto -> proto
```
// string
const a = '123';
// number
const b = 123;
// array
const c = [1,2,3];
// function
const d = function() {
	console.log('this is function')
};
// object
const e = {
	'test': 1
}
```

## 以下分別列出各型別的原型

### string
```
const a = '123';  // string
a.__proto__;  // string
a.__proto__.__proto__;  // object
a.__proto__.__proto__.__proto__;  // null
string -> string -> object -> null
```

### number

number -> number -> object -> null

### array

array -> array -> object -> null

### function

function -> function -> object -> null

### object

object -> object -> null

## reflection

一個物件可以看到自己的東西，然後改變自己的屬性和方法

## for…in

`for…in` 用法和 `Array.prototype.forEach` 很像，但他可以針對物件 (object) 或陣列 (array) 使用
```
var john = {
    firstName: 'John',
    lastName: 'Doe'
};
for (var prop in john) {
    console.log(prop + ':' + john[prop]);
}
// firstName:John
// lastName:Doe
```

prop 是自訂的變數，會重複把 john 物件的屬性存在變數中，直到沒有屬性為止

## hasOwnProperty

用來區分屬性是既有的還是本來就有的
```
//  function constructor
var Person = function (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
//  function constructor 的 prototype
Person.prototype.getFullName = function () {
  return this.firstName + ' ' + this.lastName
}
//  根據 function constructor 所建立的物件 Customer1
var Customer1 = new Person('John', 'Doe')
//  透過for...in輸出
for (var prop in Customer1) {
  console.log(prop + ': ' + Customer1[prop])
}
// firstName:John
// lastName:Doe
// getFullName: function () {return this.firstname + '' + this.lastname}
```

這裡可以發現，連繼承的屬性也一併顯示出來了

使用以下的方法就可以區分是不是繼承來的
```
for (var prop in Customer1) {
    if(Customer1.hasOwnProperty(prop) {
        console.log(prop + ': ' + Customer1[prop])
    })
}
// firstName:John
// lastName:Doe
```