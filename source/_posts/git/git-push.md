---
title: Git Remote/ Git Push
comments: false
date: 2020-07-03
categories:
    - Git
tags:
    - git
---

- `git remote`  查詢遠端分支

- `git remote -v`  查詢遠端詳細位置

- `git push` 會提交到預設的儲存庫

- `git push origin master` 指定提交的地方

> `git push origin master` 其實等於  `git push origin master:master`
> 
> 意思就是要把本地的 master 分支推上去之後，在 Server 上建立 master 分支

## 刪除遠端分支

- `git push origin :<branch>`

> 就像是推了空的內容去更新線上的 <branch> 分支的內容，也算是變相的把該分支刪除。


## 將本地與遠端儲存庫建立連結

- `git remote add origin https://github.com/[GitHub帳號]/[Repositories].git`

> 這裡 origin 只是一個代名詞，可以自己設定自定義的名稱


### 例如你的遠端節點叫做 `dragonball`，而且你想把 `cat` 分支推上去：

`git push dragonball cat`


## 將本地已存在的儲存庫(已有版控)，上傳至遠端儲存庫

- `git push -u origin master`

1. 把 master 這個分支的內容，推向 origin 這個位置。
2. 在 origin 那個遠端 Server 上，如果 master 不存在，就建立一個叫做 master 的同名分支。
3. 但如果本來 Server 上就存在 master 分支，便會移動 Server 上 master 分支的位置，使它指到目前最新的進度上。

> `-u` 表示設定 `upstream`  有設定預設值的意思，下次執行 `git push` 時，他就會被當成預設值

例如：

`git push -u origin master`:

就會把 origin/master 設定為本地 master 分支的 upstream，當下回執行 `git push` 指令而不加任何參數的時候，它就會猜你是要推往 origin 這個遠端節點，並且把 master 這個分支推上去。

### 反之，沒設定 upstream 的話，就必須在每次 Push 的時候都跟 Git 講清楚、說明白：

`git push origin master`

否則，光就執行 git push 指令而不帶其它參數的話，Git 會跟你埋怨說不知道該 Push 什麼分支以及要 Push 去哪裡：

```
$ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push —set-upstream origin master
```

### 若有建立其他分支或是標籤，在push時，請改成以下指令：

- `git push —all`

- `git push --tags`
