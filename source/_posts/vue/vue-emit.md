---
title: 父子元件通訊
comments: false
date: 2019-06-17
categories:
  - Vue
tags:
  - vue
  - javascript
---

## 父元件

```
<Pagination :pageinfo="pagination" @updatepage="getproducts" />
```

## 子元件

```
export default {
  props: ["pageinfo"],
  methods: {
    pagechild(page) {
      this.$emit('updatepage', page)
    }
  },
  computed: {
    pageinformation() {
      return this.pageinfo
    }
  }
};
<a class="page-link" href="#" @click.prevent="pagechild(page)">{{ page }}</a>
```

點下連結，觸發 pagechild，傳到父元件的 updatepage = getproducts，傳入的參數會自動帶入 getproducts，父元件要先在 data 內定義 pagination，當 pagination 改變 時，會傳入冒號後定義的變數 pageinfo，子元件也要用 prop 定義 pageinfo 來接收這個值

![](https://i.imgur.com/g5nacXo.png)
