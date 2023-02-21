---
title: display-flex
comments: false
date: 2020-07-03
categories:
    - CSS
tags:
    - css
---

# display: flex
## flex
```html
<div>
    <h2>使用flex控制份數</h2>
    <div class="flexcard">
      <div class="left">left</div>
      <div class="mid">mid</div>
      <div class="right">right</div>
    </div>
  </div>
```

> `flex: [flex-grow = 1] [flex-shrink = 1] [flex-basic = 0%]`


### flex 比例分配

```css
.flexcard {
	width: 400px;
  height: 200px;
  display: flex;
  font-size: 30px;
}

.left {
	flex: 1;
  background-color: blue;
}

.mid {
	flex: 2;
  background-color: green;
}

.right {
	flex: 3;
  background-color: red;
}

// 總合超出父元素的寬度，會按照比例分配
.left {
  flex: 100px;
  background-color: blue;
}

.mid {
  flex: 200px;
  background-color: green;
}

.right {
  flex: 300px;
  background-color: red;
}
```

![display-flex](D4C08F27-6D5A-4682-A730-5E3D22F87328.png)

### flex-shrink 縮減分配

####  當寬度小於內容總和時

```css
// 總共是 400px - 200px = 200px
// 200*(1/4) = 50px
.left {
  flex: 100px;
  background-color: blue;
}

// mid 被設定為不能縮減，所以最小就會是 200px，剩下的寬度再按照比列分配給另外兩個
.mid {
  flex: 200px;
	flex-shrink: 0;
  background-color: green;
}

// 200*(3/4) = 150px
.right {
  flex: 300px;
  background-color: red;
}
```

![display-flex](D3D57C9C-C1E6-4D9D-A4B6-007B2CE6286F.png)

#### 當寬度大於內容總和時

```css
// 總共是 1200px
// flex: 100px = flex: 1 100px; 因為 flex-grow 預設為 1
// 1200 - (100 + 200 + 300) = 600
// 600 * (1/3) = 200 多出來的各有一等份
// 100px + 200px = 300px

// 300px
.left {
  flex: 100px;
  background-color: blue;
}

// 600 * (1/3) = 200 多出來的各有一等份
// 200px + 200px = 400px

// 400px
.mid {
  flex: 200px;
	flex-shrink: 0;
  background-color: green;
}

// 600 * (1/3) = 200 多出來的各有一等份
// 300px + 200px = 500px

// 500px
.right {
  flex: 300px;
  background-color: red;
}
```

![display-flex](01FDAC58-A061-4E83-83F9-686AE4A43443.png)

### flex-grow 膨脹分配

```css
// 總共是 1200px
// flex: 100px = flex: 1 100px; 因為 flex-grow 預設為 1
// 1200 - (100 + 200 + 300) = 600
// 600 * (1/2) = 300
// 100px + 300px = 400px

// 400px
.left {
  flex: 100px;
  background-color: blue;
}

// 不膨脹也不縮小，不參與分配
// 200px
.mid {
  flex: 200px;
	flex-shrink: 0;
	flex-grow: 0;
  background-color: green;
}

// 600 * (1/2) = 300 多出來的各有一等份
// 300px + 300px = 600px

// 600px
.right {
  flex: 300px;
  background-color: red;
}
```

![display-flex](7DE6C55F-9628-4AF3-B8E1-7FBF5EF66C6B.png)


### order 排列上的比重 (優先序)

#### flex-direction: row

> `order` 越大會在越後面
> `order` 越小會在越前面

```css
.card {
  width: 600px;
  height: 200px;
  display: flex;
  font-size: 30px;
}

.left {
  flex: 100px;
  background-color: blue;
}

.mid {
  flex: 200px;
  background-color: green;
  flex-shrink: 0;
  flex-grow: 0;
}

.right {
  flex: 300px;
  order: -1;
  background-color: red;
}
```

![display-flex](97662702-FE11-4D8B-A27E-D6F0A14F17CE.png)

#### flex-direction: row-reverse

> `order` 越大會在越前面
> `order` 越小會在越後面

```css
.card {
  width: 600px;
  height: 200px;
  display: flex;
  font-size: 30px;
  flex-direction: row-reverse;
}

.left {
  flex: 100px;
  background-color: blue;
}

.mid {
  flex: 200px;
  background-color: green;
  flex-shrink: 0;
  flex-grow: 0;
}

.right {
  flex: 300px;
	order: -1;
  background-color: red;
}
```

![display-flex](A62093D1-239A-403E-A44E-F3D0CE10B344.png)

> [HTML/CSS快速入門-flexbox排版入門](https://codepen.io/frank890417/pen/Ojjgaq?editors=0100)
[圖解 Flexbox 進階屬性 | Summer。桑莫。夏天](https://cythilya.github.io/2017/04/06/flexbox-advance/)