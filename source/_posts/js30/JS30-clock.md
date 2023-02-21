---
title: CSS Clock
date: 2019-05-12
comments: false
categories:
    - JS30
tags:
    - JS30
---

# 02 — Clock

![clock-demoImage](0_dD6RkQlAXD35bd7-.png)

## 主題

畫面呈現一個時鐘

[Clock](https://des86532.github.io/javascript-30/02_JS-and-CSS-Clock/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/02_JS-and-CSS-Clock)

## 步驟

### Step1. 新增變數

先各自抓到秒針時針分針的CSS

### Step2. 建立function

1. 一個變數用來儲存現在的時間
2. 三個變數分別儲存現在時間的秒、分、時
3. 三個變數分別儲存秒的角度、分的角度、時的角度
4. 根據各自的角度變化

### Step3. 新增setInterval

`setInterval(function, milliseconds, param1, param2, …)` 這邊用每秒更新一次

### Step4. 呼叫function

## JavaScript語法&備註

### CSS:

```
<div class="clock">
    <div class="clock-face">
    <div class="hand hour-hand"></div>
    <div class="hand min-hand"></div>
    <div class="hand second-hand"></div>
    </div>
</div>
```
要設定 second-hand 的 CSS，也只需要在 style 標籤下`.second-hand{}`就可以