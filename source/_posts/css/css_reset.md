---
title: CSS Reset
comments: false
date: 2019-06-09
categories:
    - CSS
tags:
    - css
    - css reset
---

瀏覽器會有預設margin，為了讓網頁排版可以全部按照我們的程式碼執行，會在CSS檔案的最上方加入CSS Reset，讓這些瀏覽器預設的樣式歸零

![CSS Reset](0_vjLkA_8Fjj27FTJD.png)

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<style type="text/css" media="screen">
	.test1 {
		width:200px;
		height: 200px;
		background-color: blue;
	}
	.test2 {
		width:200px;
		height: 200px;
		background-color: red;
	}
</style>
<body>
	<div class="test1"></div>
	<div class="test2"></div>
</body>
</html>
```

## CSS Reset 語法 (此為個人慣用)


```
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
    display: block;
}
body {
    line-height: 1;
}
ol, ul {
    list-style: none;
}
blockquote, q {
    quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
    content: '';
    content: none;
}
table {
    border-collapse: collapse;
    border-spacing: 0;
}

* {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}
```

### box-sizing:content-box

div 設定的寬度僅為內容寬度，而內距與邊框額外加上去

### box-sizing:boder-box
div 設定的寬度就已經包含內容寬度、內距與邊框寬度

### *表示全部套用