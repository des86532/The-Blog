---
title: package.json
comments: false
date: 2020-07-03
categories:
    - Npm
tags:
    - npm
---

## npm

我們建議每一個項目都要有一個 `package.json` 文件(包描述文件，就像產品說明書一樣)
這個文件可以透過 `npm init` 來自動初始化出來

`npm install` 簡寫 `npm i`
`npm install <包名>` 簡寫 `npm i <包名>`
`npm install —save` 簡寫 `npm i -S`
`npm uninstall <包名>` 簡寫 `npm un <包名>`
`npm uninstall <包名> —save` 簡寫 `npm un <包名> -S`

### 下載第三方套件時，透過後面附加的地址下載
`npm install query —-registry=https://registry.npm.taobao.org`

### 將npm下載地址更改為`https://registry.npm.taobao.org`
`npm config set registry https://registry.npm.taobao.org`

> [npm | build amazing things](https://www.npmjs.com/)

## package.json
- 建議每個項目的跟目錄下都有一個 `package.json` 文件
- 建議執行 `npm install <包名>` 的時候都加上 `--save`這個選項，目的是用來保存依賴項信息
	- `npm install <包名>` 不會記錄在 `package.json`
	- `npm install <包名> --save` 會記錄在 `package.json` 的 `dependencies`
	- 	`npm install <包名> --save-dev` 不會記錄在 `package.json` 的 `devDependencies`
