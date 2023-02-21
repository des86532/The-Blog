---
title: Webcam Fun
comments: false
date: 2019-05-26
categories:
    - JS30
tags:
    - JS30
---

# 19 — Webcam Fun

[webcam_fun](https://des86532.github.io/javascript-30/19_Webcam-Fun/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/19_Webcam-Fun)

## 主題

利用`navigator.mediaDevices.getUserMedia`來取得視訊鏡頭影像，並透過`cavas`來達到拍照與濾鏡的效果。

## 步驟

### Step1. 啟動Local Server

這個練習需要使用到`local server`，
如果你已經有一個可在本機run起來的server可以直接使用，
或在這層資料夾底下運行`npm install`來安裝`browser-sync`，
安裝完成後可以透過指令`npm start`來啟動`localserver(預設port3000)`，
npm指令需要下載`node.js`來使用

### Step2. 取得影像

透過`navigator.mediaDevices.getUserMedia`來取得視訊影像
```
function getVideo() {
  // 取得user的視訊裝置，回傳Promise狀態
  navigator.mediaDevices.getUserMedia({
      video: true,
      audio: false
    })
    // 如果允許則把回傳的MediaStream寫進html的video tag中並播放
    .then(localMediaStream => {
      /* console.log(localMediaStream); */
      /*video.src = window.URL.createObjectURL(localMediaStream); */
      /* 2017.11更新為以下寫法URL.creatrObjectURL寫法被移除*/
      video.srcObject = localMediaStream;
      video.play();
    })
    // 當失敗時印出錯誤結果
    .catch(err => {
      console.error(`ERROR: `, err);
    })
}
```

> [MDN-MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
[HTMLMediaElement.srcObject](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/srcObject#Syntax)

### Step3. 取得視訊資料並輸出在cavas區塊中
```
function paintToCanavas() {
  // 設置寬高
  const width = video.videoWidth;
  const height = video.videoHeight;
  canvas.width = width;
  canvas.height = height;
  // 用setInterval來持續取得目前的影像資訊
  return setInterval(() => {
    // 在canvas中設置內容來源與video相同，並且X、Ｙ軸及長寬與video相同
    //context.drawImage(img,x,y,width,height);
    //x,y為position,width,height為圖的長寬
    ctx.drawImage(video, 0, 0, width, height);
  }, 16)
}
```

> [MDN-CanvasRenderingContext2D.drawImage](https://www.w3schools.com/Tags/canvas_drawimage.asp)
[HTML5-video](https://ithelp.ithome.com.tw/articles/10055442)

### Step4. 製作拍照功能！
```
function takePhoto() {
  // 拍照的音效->把音效切到第0秒並播放
  //音檔來源好像掛了
  snap.currentTime = 0;
  snap.play();
  // 利用toDataURL把canvas的內容轉為base64的圖檔資訊
  const data = canvas.toDataURL('image/jpeg');
  // 用createElemamnt來建立一個新的a元素
  const link = document.createElement('a');
  // 設置連結位置為轉圖檔後的base64位置
  link.href = data;
  // 設置連結為下載
  link.setAttribute('download', 'photo');
  // 內部新增一個預覽圖
  link.innerHTML = `<img src="${data}" alt="photo" />`;
  // 在圖片區塞入新圖片（在第一筆的位置）
  strip.insertBefore(link, strip.firstChild);
  //補充
  //<img alt>當圖片失效時，會顯示的文字 
  //<img title> 當滑鼠移動到圖片上會出現的說明文字
}
```
> [MDN-HTMLCanvasElement.toDataURL](https://developer.mozilla.org/zh-TW/docs/Web/API/HTMLCanvasElement/toDataURL)
[html dom insert before](https://www.w3school.com.cn/jsref/met_node_insertbefore.asp)

### Step5. 濾鏡效果（紅色）

再回到Step3的`paintToCanavas()`中新增：
```
function paintToCanavas() {
  // ...略
  return setInterval(() => {
    ctx.drawImage(video, 0, 0, width, height);
    // 透過getImageData取得當前canvans中所有的像素點(r,g,b,alpha的資訊)
    let pixels = ctx.getImageData(0, 0, width, height);
    // console.log(pixels)
    //輸入debugger可以把網頁動作停下來
    // debugger;
    // 製作效果
    pixels = redEffect(pixels); // 紅色濾鏡效果
    // ctx.globalAlpha = 0.8;
    // pixels = greenScreen(pixels);
    // 置入效果
    ctx.putImageData(pixels, 0, 0);
  }, 16)
}
```
並新增一個對應的濾鏡`functionredEffect()`
```
function redEffect(pixels) {
  // 透過迴圈將取回的所有像素資料跑一次，i +=4 是因為四個一組(r,g,b,alpha）
  //aplph -> (0-255; 0 是透明的，255 是完全可见的)
  //這個是每個像素點的紅色的顏色深度，console.log(pxels)可以看見每個像素點的分配
  for (let i = 0; i < pixels.data.length; i += 4) {
    // 下面組合就是單純把R(紅色)增強達到紅色濾鏡的效果
    pixels.data[i + 0] = pixels.data[i + 0] + 100;
    pixels.data[i + 1] = pixels.data[i + 1] - 50;
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5;
  }
  return pixels;
}
```
> [MDN-CanvasRenderingContext2D.getImageData](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getImageData)
[MDN-putImageData](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/putImageData)
[W3school-getImageData](https://www.w3school.com.cn/html5/canvas_getimagedata.asp)