---
title: Geolocation
comments: false
date: 2019-05-28
categories:
    - JS30
tags:
    - JS30
---

# 21 - Geolocation

![geolocation](0_luV5NMsO4d-WpbxT.png)

## 主題

利用`navigator.geolocation`來取得裝置的地理位置與速率。

[Geolocation](https://des86532.github.io/javascript-30/21_Geolocation/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/21_Geolocation)

## 步驟

### Step1. 啟動Local Server

這個練習需要使用到local server，
如果你已經有一個可在本機run起來的server可以直接使用，
或在這層資料夾底下運行`npm install`來安裝`browser-sync`，
安裝完成後可以透過指令npm start來啟動localserver(預設port3000)，
npm指令需要下載node.js來使用

### Step2. 測試

由於這個練習是需要取得定位資訊，
所以可以透過手機瀏覽器利用npm start啟動server後的內網ip來連線，
或是使用Mac的Xcode開發工具來模擬移動中的裝置(影片教學是使用後者)。

### Step3. 撰寫程式

```
// 取得HTML中的元素
const arrow = document.querySelector('.arrow');
const speed = document.querySelector('.speed-value');
// 使用watchPosition來取得使用者的地理位置及海拔、速度
navigator.geolocation.watchPosition((data) => {
  // 若成功取回，則會回傳一組Position(這裡定義名稱為data)
  console.log(data);
  // 使用coords.speed取回速度(公尺/秒)
  speed.textContent = data.coords.speed;
  // 使用coords.heading取得方位，代表偏離北方的角度，0為正北、90為正東
  arrow.style.transform = `rotate(${data.coords.heading}deg)`;
}, (err) => {
  // 錯誤回傳訊息，例如未取得定位授權時
  console.error(err);
});
```

> [MDN-Geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)
