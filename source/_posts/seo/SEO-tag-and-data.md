---
title: 語意化標籤&結構化資料
comments: false
date: 2019-06-08
categories:
    - SEO
tags:
    - seo
    - meta
---

# 【SEO】語意化標籤&結構化資料

![SEO-語意化標籤&結構化資料](1_E_KjW9-Tpp9rm-fCt79oGQ.png)

語意化標籤就是將我們人類所讀取的資料換成另一種方式顯示給電腦讀取

Micordate 是由 Google, Microsoft, Yandex, Yahoo 共同制定的規則，需要標記在網頁HTML內，主要有三種屬性

- itemscope : 用來標記內容，標記為讀取表頭資訊的對象
- itemtype : 用來分類，分類此網站為甚麼樣的內容。例如: Product, Article
- itemprop : 為分類底下的屬性

> 屬性總表：[schema.org](https://schema.org/docs/full.html)
快速入門文檔：[schema.org](https://schema.org/docs/gs.html)

## 使用範例：

我們可以給這個事物定義一個類型，比如一本書，這時就需要用到 **itemtype**，它的值是一個或多個 URL：
```
<div itemscope itemtype="http://example.com/Book">
...
</div>
```

如上的代碼片斷，我們知道它是一本書了，但是它還缺少書的信息，需要用 **itemprop** 來標示：
```
<div itemscope itemtype="http://example.com/Book">
  <div>書名：<span itemprop="name">改革歷程</span></div>
  <div>作者：<span itemprop="author">趙紫陽</span></div>
  ...
</div>
```

也許因為奇怪的設計方案，也許因為 PM 的無理取鬧，也許就是因為代碼不太好寫，佈局並不能輕易合乎要求，**itemprop** 可能散落於其它地方，這時可以使用 itemref 將它們連接起來：
```
<div itemscope itemtype="http://example.com/Book" itemref="a b">
  <div>作者：<span itemprop="author">趙紫陽</span></div>
  ...
</div>
<!-- 因為某些原因，書名與價格不在 scope 裡面 -->
<div id="a">書名：<span itemprop="name">改革歷程</span></div>
<div id="b">價格：<span itemprop="price">$12</span></div>
```

實例： [http://schema.org/Car](https://schema.org/Car)
```
<div itemscope itemtype="http://schema.org/Car">
  <h2 itemprop="name">奇瑞QQ</h2>
  <p itemprop="description">中國汽車製造商奇瑞汽車公司於2003年推出的一款微型車(...)</p>
  <img itemprop="image" href="2003_qq.png" />
  <div>
    <strong>顏色</strong>
    <span itemprop="color">黑色</span>
  </div>
  <div>
    <strong>齒輪數</strong>
    <span itemprop="numberOfForwardGears">6</span>
  </div>
  <div itemprop="vehicleEngine" itemscope itemtype="http://schema.org/EngineSpecification">
    <div>
      <strong>引擎</strong>
      <span itemprop="name">1.1 L SQR472F I4 DOHC 16v — 50 kW at 6000 rpm, 90 N·m at 3500 rpm</span>
    </div>
  </div>
  <div>
    <strong>氣囊數</strong>
    <span itemprop="airbags">4</span>
  </div>
</div>
```
## 表頭範例模板

### 1. The Minimal Template
```
Minimum Social Media Tag Template: Article
<!-- Place this data between the <head> tags of your website -->
<title>Page Title. Maximum length 60-70 characters</title>
<meta name="description" content="Page description. No longer than 155 characters." />
<!-- Twitter Card data -->
<meta name="twitter:card" value="summary">
<!-- Open Graph data -->
<meta property="og:title" content="Title Here" />
<meta property="og:type" content="article" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:image" content="http://example.com/image.jpg" />
<meta property="og:description" content="Description Here" />
```

### 2: The Standard Template
```
Standard Social Media Tag Template: Article
<!-- Place this data between the <head> tags of your website -->
<title>Page Title. Maximum length 60-70 characters</title>
<meta name="description" content="Page description. No longer than 155 characters." />
<!-- Twitter Card data -->
<meta name="twitter:card" content="summary">
<meta name="twitter:site" content="@publisher_handle">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Page description less than 200 characters">
<meta name="twitter:creator" content="@author_handle">
<-- Twitter Summary card images must be at least 120x120px -->
<meta name="twitter:image" content="http://www.example.com/image.jpg">
<!-- Open Graph data -->
<meta property="og:title" content="Title Here" />
<meta property="og:type" content="article" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:image" content="http://example.com/image.jpg" />
<meta property="og:description" content="Description Here" /> 
<meta property="og:site_name" content="Site Name, i.e. Moz" />
<meta property="fb:admins" content="Facebook numeric ID" />
```

### 3: The Full Monty
```
Full Social Media Tag Template: Article
<!-- Update your html tag to include the itemscope and itemtype attributes. -->
<html itemscope itemtype="http://schema.org/Article">
<!-- Place this data between the <head> tags of your website -->
<title>Page Title. Maximum length 60-70 characters</title>
<meta name="description" content="Page description. No longer than 155 characters." />
<!-- Schema.org markup for Google+ -->
<meta itemprop="name" content="The Name or Title Here">
<meta itemprop="description" content="This is the page description">
<meta itemprop="image" content="http://www.example.com/image.jpg">
<!-- Twitter Card data -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@publisher_handle">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Page description less than 200 characters">
<meta name="twitter:creator" content="@author_handle">
<!-- Twitter summary card with large image must be at least 280x150px -->
<meta name="twitter:image:src" content="http://www.example.com/image.jpg">
<!-- Open Graph data -->
<meta property="og:title" content="Title Here" />
<meta property="og:type" content="article" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:image" content="http://example.com/image.jpg" />
<meta property="og:description" content="Description Here" />
<meta property="og:site_name" content="Site Name, i.e. Moz" />
<meta property="article:published_time" content="2013-09-17T05:59:00+01:00" />
<meta property="article:modified_time" content="2013-09-16T19:08:47+01:00" />
<meta property="article:section" content="Article Section" />
<meta property="article:tag" content="Article Tag" />
<meta property="fb:admins" content="Facebook numberic ID" />
```

### Bonus: The Product Template
```
Product Social Media Tag Template
<!-- Update your html tag to include the itemscope and itemtype attributes. -->
<html itemscope itemtype="http://schema.org/Product">
<!-- Place this data between the <head> tags of your website -->
<title>Page Title. Maximum length 60-70 characters</title>
<meta name="description" content="Page description. No longer than 155 characters." />
<!-- Schema.org markup for Google+ -->
<meta itemprop="name" content="The Name or Title Here">
<meta itemprop="description" content="This is the page description">
<meta itemprop="image" content="http://www.example.com/image.jpg">
<!-- Twitter Card data -->
<meta name="twitter:card" content="product">
<meta name="twitter:site" content="@publisher_handle">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Page description less than 200 characters">
<meta name="twitter:creator" content="@author_handle">
<meta name="twitter:image" content="http://www.example.com/image.jpg">
<meta name="twitter:data1" content="$3">
<meta name="twitter:label1" content="Price">
<meta name="twitter:data2" content="Black">
<meta name="twitter:label2" content="Color">
<!-- Open Graph data -->
<meta property="og:title" content="Title Here" />
<meta property="og:type" content="article" />
<meta property="og:url" content="http://www.example.com/" />
<meta property="og:image" content="http://example.com/image.jpg" />
<meta property="og:description" content="Description Here" />
<meta property="og:site_name" content="Site Name, i.e. Moz" />
<meta property="og:price:amount" content="15.00" />
<meta property="og:price:currency" content="USD" />
```
## 小工具 :

[結構化資料測試工具](https://search.google.com/structured-data/testing-tool/u/0/)
[結構化資料標記協助工具](https://www.google.com/webmasters/markup-helper/u/0/)

> [前端的基礎修養：Microdata](https://lepture.com/zh/2015/fe-microdata)
[要改的地方太多了，那就改天吧](https://blog.user.today/html5-semantic-tag-and-microdata-seo/)
[moz-Must have social meta tags](https://moz.com/blog/meta-data-templates-123)
[奇寶網路：結構化資料標記](https://www.seoseo.com.tw/article_detail_208.html)