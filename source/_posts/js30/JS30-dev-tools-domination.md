---
title: Dev Tools Domination
date: 2019-05-17
comments: false
categories:
    - JS30
tags:
    - JS30
---

# 09 — Dev Tools Domination

## 主題

介紹chrome的開發工具，各種`console.`的用法

### DOM BREAK ON ..

介紹了DOM的中斷點模式，分別有三種觸發模式可選（可複選）

**subtree modifications: 當子元素點發生變化時**

**arrtibute modifications: 當元素發生變化時**

**node removal: 當元素被移除時**

使用方法為如圖，對選取的元素按下`右鍵 > Break on...` 即可。

![dev_tools_domination-image1](0_MZlppsoLJbhggnac.png)

## CONSOLE.THINGS

介紹各種 `console` 用法

### 1. console.log()

```
console.log('hello');
```

![dev_tools_domination-image2](0_wv0NVxMAyldVAyl3.png)

### 2. console.log(‘%s’, value)

可將字串中的%s顯示為指定的參數

```
console.log('Hello I am a %s string!', '
');
```

![dev_tools_domination-image3](0_gLi0sL_R7nY-J2cY.png)

### 3. console.log(‘%c’, font-style)

可將字串顯示為參數中帶入的css樣式（font系列的style)

```
console.log('%c I am some great text', 'font-size:50px; background:red; text-shadow: 10px 10px 0 blue')
```

![dev_tools_domination-image4](0_NQZ1TppS-Tvzzp1R.png)

### 4. console.warning()

顯示為警告圖示

```
console.warn('OH NOOO');
```

![dev_tools_domination-image5](0_0nQv7HbZneWQoOxq.png)

### 5. console.error()

顯示為錯誤圖示

```
console.error('Shit!');
```

![dev_tools_domination-image6](0_uZNA7-tu68XeMhFb.png)

### 6. console.info()

顯示為資訊圖示

```
console.info('Crocodiles eat 3-4 people per year');
```

![dev_tools_domination-image7](0_8MgkJcYaRzUmZ24p.png)

### 7. console.assert()

```
const p = document.querySelector('p');
    console.assert(p.classList.contains('ouch'), 'That is wrong!');
```

可以拿來測試判斷是否為真，若為false則回傳對應的錯誤訊息。

![dev_tools_domination-image8](0_ylF0cjU9qUON7ayj.png)

### 8. console.clear()

清除console的所有訊息。

補充：**Mac** 上清除的快捷鍵為 `⌘(Command)+K`、**Windows** 快捷鍵則為`CTRL+L`

### 9. console.dir()

```
console.log(p);
console.dir(p);
```

可以顯示選取物件的所有屬性，
我寫的這個範例中，console.log(p)只能返回test本身的function內容，
若使用`console.dir(p)`則可以印出p本身及其所擁有的屬性（注意屬性第一行run）。

![dev_tools_domination-image9](0_0sv_QFSWturyISvC.png)

### 10.console.groupCollapsed() & console.groupEnd()

```
const dogs = [{ name: 'Snickers', age: 2 }, { name: 'hugo', age: 8 }];
dogs.forEach(dog => {
      console.groupCollapsed(`${dog.name}`);
      console.log(`This is ${dog.name}`);
      console.log(`${dog.name} is ${dog.age} years old`);
      console.log(`${dog.name} is ${dog.age * 7} dog years old`);
      console.groupEnd(`${dog.name}`);
    });
```

可以把輸出資訊透過group包起來。

![dev_tools_domination-image10](0_BrkXwrBNer7iU1NV.png)

### 11. console.count()

```
console.count('Wes');
console.count('Wes');
console.count('Steve');
console.count('Steve');
console.count('Wes');
console.count('Steve');
console.count('Wes');
console.count('Steve');
console.count('Steve');
console.count('Steve');
console.count('Steve');
console.count('Steve');
```

用來累加出現次數。

![dev_tools_domination-image10](0_TJe2tkVc7yvo2jJv.png)

### 12.console.time() & console.timeEnd()

可以用來計算區域內執行的時間，我寫的範例是計算取回json資料的時間。

```
console.time('fetching data');
    fetch('https://api.github.com/users/wesbos')
      .then(data => data.json())
      .then(data => {
        console.timeEnd('fetching data');
        console.log(data);
      });
```

![dev_tools_domination-image11](0_LWnxBVb1efSYcRCp.png)

會在執行到 `console.timeEnd(‘fetching data’);` 時，console 出

![dev_tools_domination-image12](0_ETxITy1h34C9dZeh.png)

console.log(data);

### 13.console.table()

可以把資料整理成table格式方便瀏覽。

```
const dogs = [{ name: 'Snickers', age: 2 }, { name: 'hugo', age: 8 }];
console.table(dogs);
```

![dev_tools_domination-image13](0_EI37R-4PGpVquCvJ.png)

## 其他

還有很多可以透過開發工具來協助的，
例如監測整個網頁的讀取速度可以透過Network這個頁籤來查看，
也可以設置模擬各種網路速度、或是離線狀態等..
非常推薦觀看六角學院的免費課程，可以透過影片了解更多開發工具的操作範例。

> [六角學院-Chrome 網頁除錯功能大解密(免費)](https://www.udemy.com/course/chrome-devtools/)
[Google Dev Tool API](https://developers.google.com/web/tools/chrome-devtools/)