---
title: 用 Vue-cli 建立新專案
comments: false
date: 2019-06-16
categories:
    - Vue
tags:
    - vue
    - javascript
---

## Vue-cli 是什麼？

簡單來說它是 Vue.js 官方提供的開發工具，可用於快速開發大型單頁應用程式 (SPA)，更具體來說像是前端在開發時，常會搭配一些前端管理工具 (Gulp、Webpack…) 來處理瑣碎又重複性的工作，但往往需要大量時間去自行配置開發環境，而 Vue-cli 就像開發懶人包，可透過指令快速地建立一個立即可用的 Vue 開發環境。

目前 Vue-cli 提供了六種基本樣板，如下：
- [webpack](https://github.com/vuejs-templates/webpack)
- [webpack-simple](https://github.com/vuejs-templates/webpack-simple)
- [browserify](https://github.com/vuejs-templates/browserify)
- [browserify-simple](https://github.com/vuejs-templates/browserify-simple)
- [pwa](https://github.com/vuejs-templates/pwa)
- [simple](https://github.com/vuejs-templates/simple)

### 查詢所有樣板指令
```
$ vue list
```
大部分都是使用 webpack 樣板

### 安裝 Vue-cli 套件
Vue cli 的名稱由 `vue-cli` 改成了 `@vue/cli`。 如果你已经全局安装了舊版本的 `vue-cli(1.x 或 2.x)`，你需要通過 `npm uninstall vue-cli -g` 或 `yarn global remove vue-cli` 卸载它。
```
$ npm install -g vue-cli   //vue-cli 2.xx
$ npm install -g @vue/cli  //vue-cli 3.xx
```

安裝 Vue cli 要求 Node.js 版本在 8.9以上

### 建立 Vue 專案
```
$ vue init <樣板名稱> <專案名稱>  //vue-cli 2 
$ vue create <專案名稱>  //vue-cli 3
```

ex: vue init webpack my-project

### 環境設定

- Project name：專案名稱
- Project description：專案描述
- Author：作者
- Vue build：Runtime + Compiler 及 Runtime-only 兩種
- Install vue-router：是否安裝 vue-router
- Use ESLint to lint your code：是否使用 ESLint 來規範程式碼
- Pick an ESLint preset：Standard、Airbnb 及 none 三種
- Set up unit tests：是否加入單元測試
- Setup e2e tests with Nightwatch：是否加入 e2e 測試
- Should we run `npm install` for you after the project has been created：完成後是否自動執行 npm install

依個人需求調整

### 執行專案
```
$ npm run dev //vue-cli 2.xx
$ npm run serve //vue-cli 3.xx
$ npm run build //生成 dist 資料夾 vue-cli 2 and 3 皆可使用
```

## Vue cli 3 專用
### 使用圖形化介面
```
$ vue ui
```

輸入指令後，會開啟一個網頁，基本上可以做到指令能做的大部分的事情

>[Vue-cli 基礎入門 — 2018](https://qq7886.gitbooks.io/vue-cli-2018/content/structure/)
[vue-cli 3 文檔](https://cli.vuejs.org/zh/guide/installation.html)