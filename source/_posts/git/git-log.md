---
title: Git Log
comments: false
date: 2020-07-03
categories:
    - Git
tags:
    - git
---

## Git Log

- `git log`  檢視 git 紀錄

- `git log —-oneline` 檢視 git 紀錄，不一樣的輸出格式

- `git log -p [commit id]`  查看特定 commit 內容

- `git reflog` 顯示所有 log (包含隱藏的 log)


### 想找一位叫做 Sherly 的作者的 Commit

`git log --oneline --author=“Sherly"`



### 想找 Sherly and Eddie 的 Commit

`git log -—oneline --author=“Sherly\|Eddie”`



### 想要找 Commit 訊息裡面有在罵髒話的

`git log —-oneline --grep=“WTF”`



### 想要找 Commit 的檔案內容有提到 “Ruby” 這個字

`git log -S “Ruby”`



### 想要找今天早上 9 點 到 12點之間的 Commit

`git log —-oneline —-since=“9am” —-until="12am”`



### 想要找從 2017 年 1 月之後，每天早上 9 點 到 12點之間的 Commit

`git log --oneline --since=“9am” —-until="12am” —-after=“2017-01"`
