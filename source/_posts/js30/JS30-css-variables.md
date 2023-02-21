---
title: CSS Variables
date: 2019-05-13
comments: false
categories:
    - JS30
tags:
    - JS30
---

# 03 — CSS Variable

![CSS_Variables-demoImage](0_iD8cFJpaDLGDzkJ9.png)

## 主題

用 JavaScript 和 CSS 讓可以即時調整畫面，模糊、放大、調色。

[CSS-Variables](https://des86532.github.io/javascript-30/03_CSS-Variables/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/03_CSS-Variables)

## 步驟

### Step1. 新增變數

先抓取input的CSS

### Step2. 監聽事件

1. 給每個input添加監聽事件

2. 事件發生後，改變property

## 備註

### CSS 濾鏡參考 filter

**style.setproperty**

```
style.setProperty('padding', '15px'); /* 等同於 */
style.padding = '15px';
```

> [MDN-setProperty](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleDeclaration/setProperty)

`:root` 常用於聲明全局

**CSS ：**
```
:root { --color: #fff; }
在使用時：
img { background: var(--base); }
```

**dataset**

用 `dataset.*` 可以取出對象的 `data-*` 屬性，也等同於 `getAttribute`

```
<div id="test" data-no="123"></div>
document.querySelector('#test').dataset.no // 輸出123
document.querySelector('#test ').getAttribute('data-no'); // 輸出123
dataset.no = data-no
```

**如何用 JavaScript 改變 CSS 屬性值？**

在 JavaScript 中 `document.documentElement` 即代表文檔根元素。所以要改變全局的 CSS 變量，可以這樣寫：

```
document.documentElement.style.setProperty('--base', '#fff');
```