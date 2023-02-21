---
title: Fun With Html5 Canvas
date: 2019-05-16
comments: false
categories:
    - JS30
tags:
    - JS30
---

# 08 — Fun with HTML5 Canvas

![Fun_with_HTML5_Canvas-demoImage](0_MJZmUuU2ely4Ra5E.png)

## 主題

使用HTML5的Canvas來製作一個畫布，
透過滑鼠來繪製彩色粗細不一的線條～

[HTML5 Canvas](https://des86532.github.io/javascript-30/08_Fun-with-HTML5-Canvas/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/08_Fun-with-HTML5-Canvas)

## 步驟

### Step1

先在HTML的地方建立一個`<canvas>`的區塊，
只有width、height兩種屬性，
並設置一個變數 **ctx** 作為 canvas 的操作元素，
```
const ctx = canvas.getContext(‘2d’)
```
`getContext`用來取得渲染環境
設定顏色`strokeStyle`、樣式`lineJoin`、`lineCap`、`lineWidth`…

### Step2

接著設定變數各種待會會應用到的變數

```
const canvas = document.querySelector('#draw');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
ctx.strokeStyle = '#BADA55'; // 線條顏色
ctx.lineJoin = 'round'; // 線條連接樣式（轉角）
ctx.lineCap = 'round'; // 線條結束樣式
ctx.lineWidth = 100; // 線條寬度
let isDrawing = false; // 判斷是否執行畫圖中
let lastX = 0; 
let lastY = 0;
let hue = 0; // 色相值，在hsl中使用
let direction = true; // 判斷粗細增減用
```

### Step3

寫function來執行畫圖！

```
function draw(e) {
    // 判斷是否`isDrawing`，`false`則`return`不觸發此function
    if (!isDrawing) { return; }
    // 設定線條顏色為hsl模式，吃變數hue
    ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
    // 起始畫圖路徑
    ctx.beginPath();
    // 將路徑指針移動到X、Y點
    ctx.moveTo(lastX, lastY);
    // 將起始點與目前滑鼠位置的X、Y用線條連接起來
    ctx.lineTo(e.offsetX, e.offsetY);
    // 將線條繪製出來
    ctx.stroke();
    // 把結束點放進X、Y變數中
    [lastX, lastY] = [e.offsetX, e.offsetY];
    // 做顏色的變化效果，當色相值超過360後歸零
    hue++;
    if (hue >= 360) {
        hue = 0;
    }
    // 做線條寬度的變化效果，當寬度達到指令值得時候，切換direction的true/false
    if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
        direction = !direction;
    }
    if (direction) {
        ctx.lineWidth++;
    } else {
        ctx.lineWidth--;
    }
}
```

### Step4

接著設定滑鼠對應的addEventListener效果

```
// 當滑鼠按下時，將目前滑鼠的位置設定為變數中的X、Y並讓isDrawing為true
canvas.addEventListener('mousedown', (e) => {
    isDrawing = true;
    [lastX, lastY] = [e.offsetX, e.offsetY];
});
// 滑鼠移動中，執行function draw
canvas.addEventListener('mousemove', draw);
// 滑鼠放開，滑鼠離開 都將isDrawing改為false不觸發function draw
canvas.addEventListener('mouseup', () => isDrawing = false);
canvas.addEventListener('mouseout', () => isDrawing = false);
```

## Javascript語法&備註

### direction = !direction

學到的是透過這個方式來做true/false的切換。

### HTML5語法&備註

這篇幾乎都是使用到canvas的功能，
紀錄若要製作像這樣的畫布效果在canvas中的使用順序：

1. 定義線條樣式
(1) strokeStyle線條顏色
(2) lineWidth線條寬度
(3) lineJoin線條的轉角樣式
(4) lineCap線條的結束樣式
2. 移動順序
(1) beginPath()開啟一個新的繪製路徑
(2) moveTo()將繪製路徑的起點移動到指定的座標中
(3) lineTo()連接路徑終點到指定的座標中
(4) stroke()繪製路徑

> [MDN-CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D)