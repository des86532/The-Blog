---
title: 克服 JS 奇怪的地方- 閉包與 IIFEs
comments: false
date: 2019-06-23
categories:
    - Javascript
tags:
    - javascript
---

## Closure (閉包)
```
function greet(whattosay) {
    return function(name) {
        console.log(whattosay + ' ' + name)
    }
}
var sayHi = greet('Hi');
sayHi('Tony')  // Hi Tony
// 為何仍然可以取到 whattosay 的值
```

執行環境 greet 移除後，記憶體內的變數 whattosay 還會暫存，新的執行環境 sayHi 找不到 whattosay 就會往上一層找，這個包住所有可以取用的變數的現象，稱為 `閉包`

![closure](0_f-r3q3tHo_swfuMc.png)

```
function buildFunctions() {
	var arr = [];
	for (var i = 0; i < 3; i++) {
		arr.push(function() {
			console.log(i)
		})
	}
	return arr;
}
var fs = buildFunctions();
fs[0]();  // 3
fs[1]();  // 3
fs[2]();  // 3
```

![closure](0_wTC3NxZIJffcQ-X6.png)

```
fs[0]
// function() {
//    console.log(i)
//}
fs[1]
// function() {
//    console.log(i)
//}
fs[2]
// function() {
//    console.log(i)
//}
```

存在 fs 裡面的 function i 並沒有帶入 0,1,2，所以當呼叫到 function 時，會找到現有的 i

![closure](0_ugDh065BmIkmeD9b.png)

console.log() 不是在他所在的地方執行，而是當我們呼叫函數才執行

如果要讓輸出結果為 0,1,2 ，可以用下面兩個方法

![closure](0_N9_h3Xm1Uew6qPdr.png)

## 使用 Let
```
function buildFunctions() {
	var arr = [];
	for (var i = 0; i < 3; i++) {
	let j = i
		arr.push(function() {
			console.log(j)
		})
	}
	return arr;
}
var fs = buildFunctions();
fs[0]();  // 0
fs[1]();  // 1
fs[2]();  // 2
```

透過let，可以讓每次跑的迴圈都建立到一個新的記憶體位置，因此最後指稱到的地方會是不一樣的，於是可以輸出0, 1, 2的結果。

## 使用 IIFEs
```
function buildFunctions() {
	var arr = [];
	for (var i = 0; i < 3; i++) {
		arr.push(
			(function(j) {
			    return function() {
				    console.log(j)
		    	}
		    }(i))
		)
	}
	return arr;
}
var fs = buildFunctions();
fs[0]();  // 0
fs[1]();  // 1
fs[2]();  // 2
```
執行 `buildFunctions` 的時候，陣列裡面的匿名函式 `function(j){…}` 會直接被執行，創造新的執行環境，並且帶入變數 i ，所以每次迴圈都是新的執行環境，變數 j 會被存在不同的執行環境中，在呼叫 `fs[0]()` 的時候，也可以找到該執行環境中的變數 j

### 用法
```
function makeGreeting(language) {
    return function(firstname, lastname) {
        if (language === 'en') {
            console.log('Hello ' + firstname + ' ' + lastname);
        }
        if (language === 'es') {
            console.log('Hola ' + firstname + ' ' + lastname);
        }
    }
}
var greetEnglish = makeGreeting('en');
var greetSpanlish = makeGreeting('es');
greetEnglish('John', 'Doe');
greetSpanlish('John', 'Doe');
```
![closure](0_yGN21FjIY8sGgFmd.png)

## CALLBACK FUNCTION (回呼函數)
a function you give to another function, to be run when the other function is finished

我呼叫函數 a ，然後給他函數 b ，當 a 結束，他呼叫函數 b
```
function tellMeWhenDone(callback) {
    var a = 1000;
    var b = 2000;

    callback();
}
tellMeWhenDone(function() {
    console.log('I am done!')
});
```