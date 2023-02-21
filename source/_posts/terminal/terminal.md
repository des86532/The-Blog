---
title: Terminal
comments: false
date: 2020-07-03
categories:
    - Terminal
tags:
    - terminal
    - linux
---

## 基本指令
- `cd` 切換路徑

- `pwd` 取得目前所在位置

- `ls` 列出目前檔案列表

	- `ls -alf` 顯示詳細資訊

## 控制檔案
- `mkdir`  建立新的目錄

- `touch`  建立新的檔案

- `echo 狀態練習 > a.txt`  新增一個檔案 `a.txt` 內容為 `狀態練習`

- `cp` 複製檔案

把檔案 index.html 複製一份成 about.html：

```
cp index.html about.html
```

- `mv`  移動檔案 (做為更名使用)

把檔案 index.html 更名成 info.html：

```
mv index.html info.html
```

- `rm`  刪除檔案

刪除檔案 index.html：

```
rm index.html
```

刪除在這個目錄裡所有的 html 檔：

```
rm *.html
```

刪除資料夾 test：

```
rm -rf test
```

## 清空訊息
`ctrl + u`  清除所有正在輸入的內容，清除輸入匡
`clear` 清空 terminal
`history -c` 清除 terminal 歷史輸入訊息

## 顯示訊息
`history`  顯示歷史輸入訊息
`ps -ax` 顯示所有正在執行的工作序列
`ps -ax | grep <application_name>` 顯示正在執行的特定應用程式。ex: `ps -ax | grep chrome`
`ls` 顯示路徑下所有檔案
`ls -alf` 顯示路徑下所有檔案的詳細資訊

## 加上  &  表示背景執行
`npm run dev &` 背景執行 node，無法用 `ctrl + c` 中斷

### 中斷方法

找到 process ID (PID)，執行 `kill <PID>`，可以中斷該程序的執行
`killall <application_name>`，中斷該特定應用程式的所有工作序列

![terminal](E056EBEB-C63A-4FE1-87A7-2B86A794E4F1.png)

### 查看佔用 port 號
`netstat -an` 查看所有佔用 port 號
`netstat -an | grep 8882` 特定 port 號查詢

> Mac 的 `netstat` 指令看不見 PID

### 查看 port 號的 PID
`lsof -i:8882`  8882 port 資訊如下
![terminal](191C67B4-92B7-45E8-9D72-1305F644F836.png)

