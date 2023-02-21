---
title: 克服 JS 奇怪的地方 — 建立物件
comments: false
date: 2019-06-27
categories:
    - Javascript
tags:
    - javascript
---

## 函數建構子

一個正常的函數用來建立物件，當你在呼叫函數前面放了new，在執行環境的創造階段被產生的this變數，會指向新的空物件，當函數執行結束時，會回
傳該物件

## new

是一個運算子，用來建立新物件，this 會指向新物件

### 使用new時，發生的事

**空物件被建立 -> 呼叫函數 -> 產生this -> this指向新的空物件 -> Javascript 回傳被new運算子所建立的新物件**
```
function Person(firstname, lastname) {
    console.log(this)  // Person {}
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('This function is invoked');  // This function is invoked
}
var john = new Person('John', 'Doe');
console.log(john);  // Person {firstname: "John", lastname: "Doe"}
```

使用 new 會呼叫後面的函數

### 如果沒有使用 new
```
function Person(firstname, lastname) {
    console.log(this)  // window {}
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('This function is invoked');  // This function is invoked
}
var john = Person('John', 'Doe');
console.log(john);  // undefined
```

沒有使用 new 的話，function Person 不會回傳任何東西，所以會是 undefined

## 用函數和函數建構子設定原型

### 使用 prototype
```
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}
// 添加 getFullName 到 Person 的原型上
Person.prototype.getFullName = function() {
    return this.firstname + ' ' + this.lastname; 
}
var john = new Person('John', 'Doe');
```

### 為何要這樣做，不直接加到 function Person 裡面就好？

`因為會佔據記憶體空間`，如果加在 Person 裡面，我們新增 1000 個物件，就會有 1000個 getFullName 方法，function 就是物件，也會佔據記憶體空間，但如果是加在原型鍊上，就只會有一個，雖然有 1000 個物件，但只有一個 getFullName 方法

### 內建的函數建構子
```
let a = new String('john')
console.log(a)  // String {"john"} -> 是個物件，不是字串
'john'.length  === a.length  //true
// 'john' Javascript 會自動判定你要的是物件不是純值，所以將字串轉換，便可以取用到 length 方法，但不會自動轉換數值為物件
```

a. 可以取用到一堆能夠在字串上用的方法，這些方法沒有在 a 上，而是在 `String.prototype上`

## Object.create()

透過 `Object.create()` 可以建立一個空物件，同時可以將你帶入 `Object.create()` 的參數內容變成該物件的原型
```
var Person = {
  firstName: 'Default',
  lastName: 'Default',
  getFullName: function () {
    return this.firstName + " " + this.lastName;
  }
}
var john = Object.create(Person);
console.log(john.getFullName());   // Default Default
john.firstName = 'John';
john.lastName = 'Doe';
console.log(john.getFullName());  // John Doe
```

## function 繼承
```
function Person(firstname, lastname) {
	this.firstname = firstname;
	this.lastname = lastname;
}
Person.prototype.getFullName = function() {
	return this.firstname + ' ' + this.lastname;
}
function newPerson(height, weight, sex) {
	this.height = height;
	this.weight = weight;
	this.sex = sex;
}
newPerson.prototype = Object.create(Person.prototype)
// newPerson 的原型變成 Person
let John = new newPerson(175,60,'male')
// 原型為 Person
John.getFullName()   //  undefined undefined
John.firstname = 'John';
John.lastname = 'Doe';
John.getFullName()   // John Doe
```

當使用 Object.create 會創造一個新的空物件，在執行時會先找到物件中的屬性，找不到才會往原型找

## 如果瀏覽器不支援 Object.create()
我們會寫一些程式來填補某些瀏覽器不支援的情況，我們把這些程式稱做 polyfill。
如果我們不確定瀏覽器是不是有支援 Object.create() 的話，我們可以寫如下的 polyfill：
```
// polyfill for Object.create()
if (!Object.create) {
  Object.create = function (o) {
    if (arguments.length > 1) {
      throw new Error('Object.create implementation only accepts the first parameter');
    }
    function F() {};
    F.prototype = o;
    return new F();
  };
}
```

首先，`if(!Object.create)` 是用來判斷瀏覽器中是否有內建 `Object.create()` 的函式，如果沒有的話就會回傳 undefined ，在前面加一個 ! 這個邏輯運算子，則會把 undefined轉換成布林值，所以這段程式碼轉成中文的話，意思就是「如果 Object.create 不存在的話，則執行… 」。
當 Object.create 不存在的時候，接著會建立 Object.create 這個 function。 `if(arguments.length > 1)` 是說明如果所代入的參數超過一個的話，會在 console.log中回傳錯誤訊息。
最後會去執行 Object.create 這個函式原本會執行的內容，也就是先建立一個空的函式 `F(){ }`，然後把原本基礎物件的內容放入 F.prototype 中，最後再用函式建構式（function constructors）的方式，回傳 `new F( )` ，如此，就能夠達到 `Object.create()` 原本的效果

## Class(類別)
```
class Person {
    constructor(firstname, lastname){
        this.firstname = firstname;
        this.lastname = lastname;
    }
    getFullName(){
        return "Hello "+ this.firstname + " " + this.lastname;
    }
}
var john = new Person("John","Doe");
console.log(john);  // Person {firstname: 'John', lastname: 'Doe'}
```

在 JavaScript 中雖然和其他程式一樣使用了 class 這個關鍵字，但要注意的是 class 裡面所建立的內容仍然是個物件

在class中，我們可以使用 constructor 來建立物件，同時，我們也可以在class中放入方法，最後一樣透過 new 這個關鍵字，我們就可以建立john這個物件。要注意的是，在一個class中，只能有一個constructor

## 建立子類別 — extends
```
//  code from MDN
class Animal {
    constructor(name){
        this.name = name;
    }
    speak(){
        return this.name + ' makes a noise';
    }
}
class Dog extends Animal{
    speak(){
        return this.name + ' makes a bark';
    }
}
var ruby = new Dog("Ruby");
console.log(ruby);  // Dog {name: 'Ruby'}
ruby.speak();  // ruby makes a bark
```

## 呼叫父類別 — super

如果我們想要在子類別中，呼叫父類別的方法來使用，可以透過 super 這個關鍵字
```
//  modify code from MDN
class Animal{
    constructor(name){
        this.name = name;
    }
    speak(){
        return this.name + ' makes a noise';
    }
}
class Dog extends Animal{
    speak(){
        return this.name + ' makes a bark';
    }
    speakAnimal(){
        return super.speak();
    }
}
var ruby = new Dog("Ruby");
console.log(ruby.speak());    //    Ruby makes a bark
console.log(ruby.speakAnimal();    //    Ruby makes a noise
```