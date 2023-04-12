---
title:  迴流(reflow)、重繪(repaint)
comments: false
date: 2023-04-12
categories:
    - Web
tags:
    - web
---

## 瀏覽器的渲染過程

首先我們要了解瀏覽器的渲染過程：

1.解析 HTML,生成 DOM 樹，解析 CSS,生成 CSSOM樹

2.將 DOM樹 和 CSSOM樹結合，生成渲染樹（Render Tree）

3.渲染樹的每個元素包含的內容都是計算過的，它被稱之為佈局（layout）。瀏覽器使用一種流式處理的方法，只需要一次繪製操作就可以佈局所有的元素

4.將渲染樹的各個節點繪製到螢幕上，這一步被稱為繪製（painting）

即：解析html和css構建dom樹和cssom樹   ->   構建渲染樹（render tree）  ->   佈局render樹（layout）  ->   繪製render樹（painting）

## 迴流（reflow）

迴流又稱重排，當瀏覽器發現佈局（layout）發生了變化，這個時候就需要倒回去重新渲染，這個過程叫 迴流（reflow）。迴流幾乎是無法避免的，因為只要使用者進行了互動操作，就會發生頁面的一部分重新渲染。

## 重繪（repaint）

重繪（repaint）則是當我們改變某個元素的背景色，文字顏色，邊框顏色等不影響它周圍或內部佈局的屬性時，螢幕的一部分要重畫，但是元素的幾何尺寸和位置沒有發生改變。

需要注意的是 

display：none 會觸發迴流（reflow），而

visibility：hidden 屬性表示隱藏元素，元素任然佔據著佈局空間，並沒有改變佈局和幾何尺寸，所以只會觸發 重繪（repaint）

> 哪些css屬性會影響畫面重繪: [CSS Triggers List](https://csstriggers.com/)

## 何時引起迴流（reflow）

現代瀏覽器會對迴流做優化，它會等到足夠數量的變化發生，再做一次批處理迴流

1.頁面的第一次渲染（初始化）

2.DOM樹的變化（如：增刪節點）

3.Render樹的變化（如：padding 改變）

4.瀏覽器視窗的尺寸變化 resize

5.fontsize的變化

6.獲取元素的某些屬性

瀏覽器為了獲取正確的最新屬性值也會提前觸發迴流，這些屬性包括：

offsetLest、 offsetTop、 offsetWidth、 offsetHeight、 scrollTop/Left/Width/Height、

clientTop/Left/Width/Height、 呼叫了 getComputedStyle()。

## 何時引起重繪（repaint）

背景色、顏色的改變等會引起重繪（repaint）

## 迴流和重繪的區別

當元素的位置或者幾何尺寸發生改變時會觸發 迴流 ，

而當背景色和顏色的改變會引起 重繪 ，

注意：迴流一定會發生重繪，而重繪不一定發生迴流！

## 為了優化效能，我們需要減少迴流、重繪的觸發次數

1.用 transform 做形變和位移可以減少迴流

2.避免組個修改節點樣式，儘量一次性修改

3.可以將需要多次修改的 DOM 元素設定 display：none ，操作完再顯示（因為隱藏元素不在 render 樹內，因此修改隱藏元素不會觸發迴流重繪）

4.避免多次讀取某些屬性

5.通過絕對位移將複雜的節點元素脫離文件流，形成新的 Render Layer,降低迴流成本

> [【高效能JS】重繪、重排與瀏覽器優化方法 | IT人 (iter01.com)](https://iter01.com/472989.html)