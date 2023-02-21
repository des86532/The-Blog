---
title: Flex-Panel-Gallery
date: 2019-05-14
comments: false
categories:
    - JS30
tags:
    - JS30
---

# 05 — Flex 實現可伸縮的圖片牆

![Flex-Panel-Gallery](0_y6DLMvPEiGfMmZbG.png)

## 主題

點擊任一張圖片，使圖片放大，兩側文字向內滑入

[Flex-Panel-Gallery](https://des86532.github.io/javascript-30/05_Flex-Panel-Gallery/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/05_Flex-Panel-Gallery)

## 步驟

1. 抓取所有panel
2. 建立function
3. 設定click、transitionend的監聽事件

## CSS 部分

要使用flexbox需要`display:flex`才可以使用flexbox的語法

> [MDN-flexbox](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)

`>`:
直接碰到該標籤的才會變化

`*` :
所有的意思

> [CSS選擇器](https://pjchender.blogspot.com/2015/07/css.html)

## 備註

### flex-direction:

可以改變方向，為直立或是橫列

### flex: flex-grow｜flex-shrink｜flex-basis:

以下兩個相同:

flex: auto
flex: 1 1 auto;

以下兩個相同:

flex: none;
flex: 0 0 auto;

### flex-grow:

預設值為0,表示寬度就是元素內容的寬。而從1開始，數字愈大，占的寬度比例也會愈大。

### flex-shrink:

flex-shrink跟flex-grow相反，是表示當父元素寬度空間不足時，子元素壓縮的比例。預設值為 1，但設為 0 同樣表示不壓縮，數字愈大，被壓縮的幅度就愈大。

### flex-basis:

可以在flex-basis設定寬度或高度，設定好後會自動分配該區塊的寬度或高度。

### justify-content:

水平對齊

### align-content:

垂直對齊，與align-items的不同是，align-items是針對單行的處理，多行元素用align-content