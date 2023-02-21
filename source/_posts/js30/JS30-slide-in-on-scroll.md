---
title: Slide in on Scroll
comments: false
date: 2019-05-21
categories:
    - JS30
tags:
    - JS30
---

# 13 — Slide in on Scroll

![Slide_in_on_Scroll-demoImage](0_TFpHtpMtG8Zm-Uru.png)

## 主題

這篇介紹當滾動視窗到定點時動畫滑入圖片的效果

[Slide in on Scroll](https://des86532.github.io/javascript-30/13_Slide-in-on-Scroll/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/13_Slide-in-on-Scroll)

## 步驟

### Step1. 基礎設定

作者已經在所有的圖片中加入了待會會用到的class :

1. align-right / align-left : 滑入效果用（左/右）
2. slide-in : JavaScript抓取用，並已經將相關的動畫滑入效果寫好。

### Step2. 建立觸發條件,並監聽滾動事件

目的是使滾動視窗到定點時顯示效果，
所以要監聽的是整個視窗，用window，事件選用scroll，
但是如果單純使用scroll來操作的話，每次的畫面滾動都會有大量事件被觸發，
會對效能上造成影響，所以作者多寫了一個debounce來使觸發間隔為20毫秒以上：
```
function debounce(func, wait = 20, immediate = true) {
    var timeout;
    return function () {
    var context = this, args = arguments;
    var later = function () {
        timeout = null;
        if (!immediate) func.apply(context, args);
    };
    var callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    if (callNow) func.apply(context, args);
    };
}
```
所以監聽事件就會寫成`window.addEventListener('scroll', debounce(checkSlide));`。

### Step3. 設定觸發後的事件內容

在一開始先取得所有`.slide-in`的圖片元素，使用`querySelectorAll`，
```
const sliderImages = document.querySelectorAll('.slide-in');
```
接著編寫每次`scroll`觸發的`checkSlide` function:
```
function checkslide(e) {
    sliderImages.forEach(sliderImage => {
    // window.scrollY = 捲動到最上面的時候為0，最上面為起始點，往下捲動逐漸增加   
    // window.innerHeight = 為當前畫面的高度，除非調整視窗大小，不然不會改變
    const slideInAt = (window.scrollY + window.innerHeight) 
    // 元素距離外層容器上方的距離  + 元素的高度
    const imagebottom = sliderImage.offsetTop + (sliderImage.height /2);
    //這個寫法的話，有經過的圖片全部都會顯示
    //影片中的教學，超過畫面太多的圖片會被remove active
    if(slideInAt > imagebottom) {
        sliderImage.classList.add('active')
    }
    })
}
```

## JavaScript語法備註

### Window.scrollY

目前瀏覽器視窗已滾動的Y軸（垂直位置）

> [MDN-Window.scrollY](https://developer.mozilla.org/en-US/docs/Web/API/Window/scrollY)

### Window.innerHeight

目前瀏覽器視窗的高度

> [MDN-Window.innerHeight](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerHeight)

### HTMLElement.offsetTop

返回指定元素相對於有父元素(offsetParent)中的頂端位置，
以此練習來說，sliderImage的父元素就是window。

### HTMLElement.dataset

透過`dataset`可以取回在HTML中設置的`data-*`內容，
**注意使用dataset時property不用再將加上data-開頭**，例如

```
<div class="test" data-greet="hi"></div>
document.querySelector('.test').dataset.greet; // hi
```

> [MDN-HTMLElement.dataset](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/dataset)

