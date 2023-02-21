---
title: Array-methods
comments: false
date: 2019-11-19
categories:
    - Javascript
tags:
    - javascript
---

![array-methods](20106426psTRBiwFBt.png)

## 預設資料
```javascript
const temp = [1, 2, 3];
```

### Array.prototype.push(elementN)
```javascript
const newArray = temp.push(6);
console.log(temp);  	   // [1, 2, 3, 6]
console.log(newArray);  // 4
```

`push` 後會回傳 `length`

### Array.prototype.unshift(elementN)
```javascript
const newArray = temp.unshift(6);
console.log(temp);      // [6, 1, 2, 3]
console.log(newArray);  // 4
```

`unshift` 後會回傳 `length`

### Array.prototype.pop()
```javascript
const newArray = temp.pop();
console.log(temp);   // [1, 2]
console.log(newArray);  // 3
```

移除原本陣列`最後面`的第一個值，回傳被移除掉的那個值

### Array.prototype.shift()
```javascript
const newArray = temp.shift();
console.log(temp);  // [2, 3]
console.log(newArray);  // 1
```

移除原本陣列`最前面`的第一個值，回傳被移除掉的那個值

### Array.prototype.concat()
```javascript
	const newArray = temp.concat([4, 5, 6])
	console.log(temp);  // [1, 2, 3]
	console.log(newArray);  // [1, 2, 3, 4, 5, 6]
```

把兩個陣列合併在一起，並回傳新陣列

## 預設資料
```javascript
const inventors = [
    { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
    { first: ‘Isaac’, last: 'Newton', year: 1643, passed: 1727 },
    { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
    { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
    { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
    { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
    { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
    { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
    { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
    { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
    { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
    { first: ‘Hanna’, last: 'Hammarström', year: 1829, passed: 1909 }
];
```

### Array.prototype.findIndex(item, index, array)
```javascript
const result = inventors.findIndex(item => {
	return item.year > 1870
});

const result1 = inventors.findIndex(item => {
	return item.year > 2000
})

console.log(result);    // 0
console.log(result1);   // -1
```

回傳`第一個`符合條件的位置 index，若都找不到則回傳 `-1`

### Array.prototype.find(item, index, array)

```javascript
const result = inventors.find(item => {
	return item.year > 1870
});

const result1 = inventors.find(item => {
	return item.year > 2000
})

console.log(result);    // { first: ‘Albert', last: 'Einstein', year: 1879, passed: 1955 }
console.log(result1);   // undefined
```

回傳一個值，且是`第一個`抓到條件為 true 的值，若都找不到則回傳 `undefined`

### Array.prototype.filter(item, index, array)

```javascript
const result = inventors.filter(item => {
	return item.year > 1870
});

const result1 = inventors.filter(item => {
	return item.year > 2000
})

console.log(result);    
// { first: ‘Albert', last: 'Einstein', year: 1879, passed: 1955 }
// {first: "Katherine”, last: “Blodgett", year: 1898, passed: 1979}
// {first: "Lise", last: “Meitner”, year: 1878, passed: 1968}
console.log(result1);   // undefined
```

回傳一個陣列，只要條件為 true 的就會包含在此陣列，適合拿來搜尋

### Array.prototype.forEach(item, index, array)

```javascript
const forEachInventors = inventors.forEach(item => {
    return item.year > 1870
})
console.log(forEachInventors) // undefined, 不會 return 東西
 
inventors.forEach(item => {
    item.year =  1870
})
console.log(inventors) // 全部 12 個的 year = 1870
```

forEach 不會回傳任何東西，單純只執行原本陣列裡的事

### Array.prototype.map(item, index, array)

```javascript
const mapInventors = inventors.map(item => {
	return item.year > 1870
})
console.log(mapInventors)
//  [true, false, false, false, false, false, false, true, false, false, true, false]

const mapInventors2 = inventors.map(function(item){
    if(item.year > 1870){
        return `${item.year}歲`;
    }
    return false;
})
console.log(mapInventors2) // ["1879歲", false, false....]
// 不論是空的或是 false，都會回傳
```

將條件運算後重新組合回傳一個數量等於 array.length 的陣列，陣列內容只有 true & false

## Array.prototype.some(item, index, array)
```javascript
const someInventors = inventors.some(function(item){
    return item.year > 1870;
})
console.log(someInventors)  // true
```

回傳一個 Boolean，只要部分符合就回傳 true。它不會 Array 全部找完，只要一找到相符的直就跳出

## Array.prototype.every(item, index, array)

```javascript
const everyInventors = inventors.every(function(item){
    return item.year > 1870;
})
console.log(everyInventors)  // false
```

回傳一個 Boolean，需要全部符合才回傳 true，部分符合會回傳 false

## Array.prototype.reduce(accumulator, currentValue, currentIndex, array [, initialValue])

```javascript
const special = [8, 0, 14];
const reduceArray = special.reduce(
function(accumulator, currentValue, currentIndex){
  console.log('accumulator', accumulator);  // 下圖
  console.log('currentValue', currentValue);  // 下圖
  console.log('currentIndex', currentIndex);  // 下圖
    return accumulator + currentValue;
})
console.log(reduceArray) // 22
console.log(special) // 原本的 array
```

![methods-reduce](625D851B-F6CE-4577-AC39-C0874332080E.png)

```javascript
const special = [8, 0, 14];
const reduceArray = special.reduce(
function(accumulator, currentValue, currentIndex, initialValue){
  console.log('accumulator', accumulator);  // 下圖
  console.log('currentValue', currentValue);  // 下圖
  console.log('currentIndex', currentIndex);  // 下圖
	console.log('initialValue', initialValue);  // 下圖
    return accumulator + currentValue;
}, 30)
console.log(reduceArray) // 52
console.log(special) // 原本的 array
```

![methods-reduce](C7FCB7BD-93A8-4151-9A98-59FD4C90A0E4.png)

## Array.prototype.sort(compareFunction)
特別要提得是，< 10 的 array 會是 stable 排序法， > 10 會是不穩定的排序法

```javascript
const fruit = ['cherries', 'apples', 'bananas'];
fruit.sort(); // ['apples', ‘bananas’, ‘cherries’]
 
const scores = [1, 10, 21, 2];
scores.sort(); // [1, 10, 2, 21]
// 在 unicode 裡 32(數字2） > 31(1 開頭的任何數字)
 
const things = ['word', 'Word', '1 Word', '2 Words'];
things.sort(); // ['1 Word', '2 Words', 'Word', 'word']
```

```javascript
function compare(a, b) {
  // 若 a 比較小 排序就是 a, b
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  // 若 a 比較大 排序就是 b, a  就是位置會對調
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // 若一樣
  return 0;
}

const a  = [2, 4,6,7,11,75,33,64,356,86] ;

const result = a.sort((a,b) => {
	return a - b
})

console.log(result); // [2, 4, 6, 7, 11, 33, 64, 75, 86, 356]

const result1 = a.sort((a,b) => {
	return b - a
})

console.log(result1);  // [356, 86, 75, 64, 33, 11, 7, 6, 4, 2]
```

## Array.prototype.reverse()
```javascript
const a = ['one', 'two', 'three'];
console.log(a.reverse()); // [‘three', 'two', ‘one']
console.log(a);  // [‘three', 'two', ‘one']
```

會改變原來的陣列 !!

## Array.prototype.slice(start, end)
```javascript
const my_array = [5, 1, 3, 8, 6, 0]
const sliceArray = my_array.slice(2,5);
console.log(sliceArray); // [3, 8, 6]
console.log(my_array); // [5, 1, 3, 8, 6, 0] 不會變

const sliceArray1 = my_array.slice(2);
console.log(sliceArray1); // [3, 8, 6, 0]

const sliceNegativeArray = my_array.slice(-1);
console.log(sliceNegativeArray); // [0]
```

Slice 就是切，把 array 頭尾切兩刀。回傳在 start(包含) 跟 end 之間的陣列。經過 slice 後原本陣列不會改變

## Array.prototype.splice(start, deleteCount, item1, item2, …)

```javascript
// 假如只有 splice(start)，這時會跟 slice 行為一樣
const spliceArray1 = my_array.splice(3)
console.log(spliceArray1); // [8, 6, 0] 被移掉的值
console.log(my_array); // [5, 1, 3]，index 3 後面都移掉
 
//一樣可以接受負數
const spliceArray2 = my_array.splice(-1)
console.log(spliceArray2); // [0]
console.log(my_array); // [5, 1, 3, 8, 6] 倒數 1 個後面都移掉
 
// splice(start, deleteCount 抓幾個)
const spliceArray3 = my_array.splice(1, 3)
console.log(spliceArray3); // [1, 3, 8]
console.log(my_array); // [5, 6, 0]
// 從 index 1 開始抓 3 個 把中間值移掉
 
// splice(start, deleteCount 抓幾個, 插入 item)
const spliceArray4 = my_array.splice(1, 3, ‘Hana’, ‘Mike’)
console.log(spliceArray4); // [1, 3, 8]
// 不會有 'Hana', 'Mike' 字眼出現
 
console.log(my_array); // [5, "Hana", "Mike", 6, 0]
// 從 index 1 開始抓 3 個 把中間值移掉 並塞進 “Hana”, “Mike”
```

回傳被移掉的值放在陣列裡。

splice 乍看下跟 slice 有點像，但其實幾乎完全相反(驚)。slice 是取 start end 裡的東西，而 splice 是 return 中間被移掉的東西， splice 會影響原本陣列。

新增的 item1、item2 只會影響本來陣列，並不會回傳到新的陣列裡

Splice 是拼接的意思你不但可以切掉某塊還可以塞東西進去。

## Array.prototype.indexOf(searchElement)
```javascript
const indexArray = my_array.indexOf(3) // 2
console.log(indexArray) // 2
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
const indexArray2 = my_array.indexOf(100)
console.log(indexArray) // -1
```

回傳所在位置的 index，如果找不到會回傳 -1，不會改變原陣列

## Array.prototype.join(separator)
```javascript
const joinArray = my_array.join(‘-')
console.log(joinArray) // ‘5-1-3-8-6-0’
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
const joinArray2 = my_array.join(‘')
console.log(joinArray2) // '513860'
```

把所有陣列裡的值加上 separator，回傳一個字串，不會改變原陣列

## Array.prototype.includes(searchElement, fromIndex)
```javascript
const my_array = [5, 1, 3, 8, 6, 0]
const includesArray = my_array.includes(3)
console.log(includesArray) // true
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
my_array.includes(5,3) // false 從 index 第三個之後找 5
```

看 searchElement 有沒有在 array 裡，回傳 Boolean，不會改變原陣列

# 總結

![result](6ACAAF72-B003-46F6-B7AB-DC848B382AEE.png)

# 參考
> [陣列 Array - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10213787)
> [GitHub - tooto1985/js-array-operations: 20 kinds of methods to get to know a JavaScript array operations.](https://github.com/tooto1985/js-array-operations)
