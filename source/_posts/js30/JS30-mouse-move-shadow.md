---
title: Mouse Move Shadow
comments: false
date: 2019-05-23
categories:
    - JS30
tags:
    - JS30
---

# 16 — Mouse Move Shadow

![mouse_move_shadow](0_LFqekGOPc0xBWcaW.png)

## 主題

透過textShadow讓文字的陰影隨滑鼠位置偏移，
並稍微帶到ES6的解構賦值的用法。

[Mouse Move Shadow](https://des86532.github.io/javascript-30/16_Mouse-Move-Shadow/index.html)
[Github](https://github.com/des86532/javascript-30/tree/master/16_Mouse-Move-Shadow)

## 步驟

### Step1. 設定目標區域與基本偏移量

抓取HTML中的 **hero** 與 **woah** 做為目標區域
設定基本偏移基準 **walk = 600**

### Step2. 建立觸發條件與事件

設定 `hero.addEventListener('mousemove', shadow)`
觸發事件備註：
```
function play(e) {
  // 透過解構賦值取得並設定資訊
  const { offsetHeight: height,
          offsetWidth: width } = hero;
  //等同於這樣
    const width = hero.offsetWidth;
    const height = hero.offsetHeight;
  let { offsetX: x,
        offsetY: y  } = e;
  //等同於這樣
    let x = e.offsetX
    let y = e.offsetY  
  // 如果在目標區域外，則在加上目標座標值
  if (this !== e.target) {
    x = x + e.target.offsetLeft;
    y = y + e.target.offsetTop;
  }
  // 四捨五入最終偏移值
  const xWalk = Math.round((x / width * walk) - (walk/2));
  const yWalk = Math.round((y / height * walk) - (walk/2));
  console.log(xWalk, yWalk);
  // 使用textShadow來設定文字陰影
  woah.style.textShadow = `
    ${xWalk}px ${yWalk}px 0px rgba(0, 0, 0, 0.5),
    ${xWalk * -1}px ${yWalk}px 0px rgba(0, 0, 0, 0.5),
    ${yWalk}px ${xWalk * -1}px 0px rgba(0, 0, 0, 0.5),
    ${yWalk * -1}px ${xWalk}px 0px rgba(0, 0, 0, 0.5)
    `
}
```

## Javascript語法&備註

解構賦值(**Destructuring assignment**)
透過解構賦值，可以把直接把物件/陣列中的值塞入變數中，
擷取一小段程式碼做說明：
```
// 下面這段等同於 
const height = hero.offsetHeight;
const { offsetHeight: height } = hero;
// 下面這段等同於 
let x = e.offsetX;
let { offsetX: x } = e;
```

> [MDN-Destructuring_assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

### Math.round

可以將內容的數值進行四捨五入的動作。

> [MDN-Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

## CSS語法備註
```
/* offset-x | offset-y | blur-radius | color */
text-shadow: 1px 1px 2px black;
```

> [MDN-text_shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow)
