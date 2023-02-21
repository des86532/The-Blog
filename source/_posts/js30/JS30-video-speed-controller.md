---
title: Video Speed Controller
comments: false
date: 2019-06-03
categories:
    - JS30
tags:
    - JS30
---

# 28 — Video Speed Controller

## 主題

製作一個可控制影片速率的拉把。
這個練習可以算是延續[「JS30紀錄＆心得」11 — Custom Video Player]
主要是把原本`input range`改成`div`的滑鼠事件監聽。

[Video Speed Controller](https://des86532.github.io/javascript-30/28_Video-Speed-Controller/index.html)

[Github](https://github.com/des86532/javascript-30/tree/master/28_Video-Speed-Controller)

## 步驟

### Step1. 取得頁面元素
```
const speed = document.querySelector('.speed');
const bar = speed.querySelector('.speed-bar');
const video = document.querySelector('.flex');
```
### Step2. 建立滑鼠移動監聽與事件
```
/** 滑鼠移動事件 **/
function handleMove (e) {
  // 取得觸發點位置（滑鼠位於整頁頂端的Y軸定位-speed框到整頁頂端的距離）
  const y = e.pageY - this.offsetTop;
  // 設定百分比(y / speed框的高度)
  const percent = y / this.offsetHeight;
  const min = 0.4;
  const max = 4;
  // 用Math.round來計算取得四捨五入的百分比值
  const height = Math.round(percent * 100) + '%';
  // 取得播放速率(0.4一跳，最多4倍速)
  const playbackRate = percent * (max - min) + min;
  // 調整speed-bar的樣式高度
  bar.style.height = height;
  // 用toFixed(2)來設定最多取得小數點後兩位，顯示於speed-bar上
  bar.textContent = playbackRate.toFixed(2) + 'x';
  // 控制影片的速率
  video.playbackRate = playbackRate;
}
function barchange(e) {
  //畫面最上方到滑鼠位置的距離 - 畫面最上方到bar的最上方 = 藍色的部分
  const height = e.y - speed.offsetTop
  //(藍色的部分 / 整個bar的高度(410))*100 = 百分比
  const barheight = (height / speed.offsetHeight) * 100
  //4 * 比例 = bar上顯示的數字
  const barnumber = 4 * (height / speed.offsetHeight)
  bar.style.height = barheight + '%'
  //取小數點後兩位
  bar.innerText = Math.round(barnumber * 100) / 100 + 'x'
  video.playbackRate = Math.round(barnumber * 100) / 100
}
```