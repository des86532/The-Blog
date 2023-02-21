---
title: 解決安裝權限不足
comments: false
date: 2019-06-14
categories:
    - Npm
tags:
    - npm
---

## npm 全域安裝提示權限不足的錯誤

npm WARN checkPermissions Missing write access to /user/local/lib/node_modules

![npm-解決安裝權限不足](0_tVAi3-9yDw0lRh8S.png)

## 解決方法

修改 npm 所安裝目錄的權限

```
sudo chown -R $USER /usr/local
```
會要求輸入密碼，登入密碼

## 查看目錄是否已經切換權限
```
ls -l /usr/local
```

![npm-解決安裝權限不足](0_Mru0X6_uz2gDKV6U.png)

紅色區域本來會是 root ，切換權限後會變為自己主機的名稱
