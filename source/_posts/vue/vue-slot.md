---
title: slot 介紹
comments: false
date: 2019-06-18
categories:
    - Vue
tags:
    - vue
    - javascript
---

## 沒有插槽
```
<h2>沒有插槽可替換的狀態</h2>

<test>
  <p>這段訊息會消失。</p>
</test>

<script type="text/x-template" id="test">
<div class="alert alert-warning">
  <h6>我是一個元件</h6>
  <p>這沒有插槽。</p>
  //如果這裡 p 標籤改為 slot 標籤，訊息就會被取代為 "這段訊息會消失"
</div>
</script>
```

元件中沒有 slot 標籤，所以傳入此元件的`<p>這段訊息會消失。</p>` 就不見了，因為沒地方可以插入。

## 有插槽
```
<h2>Slot 基礎範例</h2>

  <test></test>   //這段會顯示slot預設的內容，"如果沒有內容，則會顯示此段落"

  <test>
    <p>這段會取代原本的 Slot。</p>
  </test>
//p標籤，會取代 template 中 slot 的內容


<script type="text/x-template" id="test"> // 引入組件
<div class="alert alert-warning">
  <h6>我是一個元件</h6>
  <slot>
    如果沒有內容，則會顯示此段落。
  </slot>
</div>
</script>
```

元件中有 slot 標籤，且有預設的內容，所以如果沒有傳入任何東西，就會顯示`slot標籤`預設的 `如果沒有內容，則會顯示此段落。`，
有傳入的話 slot標籤內的預設內容就會被取代，顯示 `這段會取代原本的 Slot。`。

## 具名插槽
```
<h2>具名插槽</h2>
  <named-slot-component></named-slot-component>

  <named-slot-component>
    <header slot="header">替換的 Header</header>  //slot header
    <template slot="footer">替換的 Footer</template>  //slot footer
    <template slot="btnString">按鈕內容</template>  //slot btnString
    <p slot>其餘的內容</p>
  </named-slot-component>

<script type="text/x-template" id="namedSlotComponent">
<div class="card my-3">
  <div class="card-header">
    <slot name="header">這段是預設的文字</slot>  //name header
  </div>

  <div class="card-body">
    <slot>
      <h5 class="card-title">Special title treatment</h5>
      <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    </slot>
    <a href="#" class="btn btn-primary">
      <slot name="btnString">spanGo somewhere</slot>  //name btnString
    </a>
  </div>

  <div class="card-footer">
    <slot name="footer">這是預設的 Footer</slot>  //name footer
  </div>
</div>
</script>
```

javascript 中 slot 的 name 屬性為自定義的名稱，ex: header、btnString、footer，但是需對應到元件標籤內的 slot 屬性，為指定替換的對象。