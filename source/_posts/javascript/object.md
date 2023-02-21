---
title: 克服 JS 奇怪的地方 — 物件
comments: false
date: 2019-06-22
categories:
    - Javascript
tags:
    - javascript
---

## NAMESPACE (命名空間)

a container for variables and functions
typically to keep variables and functions with the same name separate

Javascript 沒有命名空間，也不需要，物件可以代替命名空間
```
var objectLiteral = {
    firstname: 'Mary',
    isAProgrammer: true
}
console.log(JSON.stringify(objectLiteral));   // json
var jsonValue = JSON.parse('{ "firstname": "Mary", "isAProgrammer": true}');
console.log(jsonValue)  // object
```

## FIRST CALSS FUNCTION (一級函數)

EVERYTHING YOU CAN DO WITH OTHER TYPES YOU CAN DO WITH FUNCTION
assign them to variables, pass them around, create them on the fly.
```
function greet() {
    console.log('hi');
}
greet.language = 'english';
console.log(greet)  // function
console.log(greet.language) // english
```

**function 也是 object**

![object](0_HCdP2iVGbMKN8Mhh.png)

## EXPRESSION (表示式)

A UNIT OF CODE THAT RESULTS IN A VALUE
it doesn’t have to save to a variable.

表示式會回傳一個值，不需要存在變數中也可以得到結果
```
console.log(1+2)  // 3
```

## statement (陳述式)
陳述句不會回傳任何值， if 就是陳述句

### 函數陳述句
```
function greet() {
    console.log('h1')
}
```

### 函數表示式
```
var anonymousGreet = function() {
    console.log('h1')
}
anonymousGreet(); // h1
```

### By value & By reference

![object](0_g6T4wq59aBld1ufc.png)
```
// by value (primitives)
var a = 3;
var b;
b = a;
a = 2;
console.log(a);  // 2
console.log(b);  // 3
// by reference (all objects (including functions))
var c = { greeting: 'hi' };
var d;
d = c;
c.greeting = 'hello'; // mutate
console.log(c);  // Hola
console.log(d);  // Hola
// by reference (even as parameters)
function changeGreeting(obj) {
    obj.greeting = 'Hola'; // mutate   
}
changeGreeting(d);
console.log(c);  // Hola
console.log(d);  // Hola
// equals operator sets up new memory space (new address)
c = { greeting: 'howdy' };
console.log(c);  // howdy
console.log(d);  // Hola
```

## argument(參數)
`arguments`其實就是`parameters`的意思，也就是說，arguments會包含所有你放入function中的參數值。
```
function person(name, age, sex, ...other) {
		console.log(arguments)
		console.log(other)
		console.log(arguments[0])
	}
	person('Mike', 123, 'male', 'para1', 'para2')
```

![object](0_ODiFG8b03q4iEG_E.png)

`arguments回傳的值是斜體的 [ ]` ，看起來很像陣列(array-like)，但它並不是真的陣列！所以arguments回傳的值並不具備所有陣列所具有的特徵。

## 展開運算子 spread(…)
把函數中許多的參數 (arguments) 或陣列中許多的元素 (elements) 形成一個新的變數。也不是真的陣列 (array-like)，斜體的 [ ]
傳入非預設的參數也可以取得，存放在變數中 (array-like)

## 立即呼叫的函數表示式(IIFEs)
```
// using an Immediately Invoked Function Expression (IIFE)
var greeting = function(name) {
    return 'Hello ' + name;
}('John');
console.log(greeting);   // Hello John
console.log(greeting()); // error greeting is not a function
var firstname = 'John';
(function(name) {
    var greeting = 'Inside IIFE: Hello';
    console.log(greeting + ' ' + name);
}(firstname));
```