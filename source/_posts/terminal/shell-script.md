---
title: shell-script
comments: false
date: 2020-07-03 16:12:37
categories:
    - Terminal
tags:
    - terminal
    - linux
---

> Shell Script主要用途就是用來協助使用者在UNIX or Linux環境上, 以更方便, 更自動化的方式來執行想要執行的指令

## Vue 專案內
`shell script` 預設有些參數可以使用：
- $?：表示你輸入的上個指令的狀態，一般情況下為 0，表示成功，不正常離開的不會是 0
- $0：shell script的檔名
- $1：表示輸入的第一個參數
- $2：表示第二個參數

`npm run BUILD_DEV`：
`echo $0`：`./deploy.sh`
`echo $1`：`dev`

`sh deploy.sh dev`：
`echo $0`：`deploy.sh`
`echo $1`：`dev`

### package.json
這邊先將 `command line` 環境更改為 `bash`，才開始執行 `shell script`
```json
“BUILD_DEV”: “bash ./deploy.sh dev",
“BUILD_STAGE”: “bash ./deploy.sh stage”,
“BUILD_MASTER”: “bash ./deploy.sh master”
```
### deploy.sh
```shell
#!/bin/zsh
BuildFolderName=dw_front_bo_ag

SourceFolder=$(pwd) // $() 可以執行裡面指令
BuildFolder=$(cd ..;cd $BuildFolderName; pwd)

npmDev=dev
npmStage=Stage
npmMaster=build

branchDev=Develop
branchStage=Stage
branchMaster=master

function Building() {
  echo "building..."
  npm run $1
  # command line status code === 0 的時候，即為成功
  if [ "$?" != 0 ];
  then
    echo "ERROR!!! 打包失敗！發布已中斷！"
    exit
  fi
  echo “change to buildFloder..."
  cd $BuildFolder
  echo “checkout branch $2…”
  git checkout $2
  echo “remove all files..."
  rm -rf $BuildFolder/*
  echo "copying files to building folder..."
  cp -R $SourceFolder/output/$1/* $BuildFolder
  echo "git add..."
  git add .
  echo "git commit..."
  git commit -m "$(date +"%D %T")"
  echo "git pull..."
  git pull
  echo "git push..."
  git push origin $2
}

if [ "$1" = "$npmDev" ];
then
  Building $npmDev $branchDev
elif [ "$1" = "$npmStage" ];
then
  Building $npmStage $branchStage
elif [ "$1" = "$npmMaster" ];
then
  Building $npmMaster $branchMaster
fi
```

> [鳥哥的 Linux 私房菜 -- 第十二章、學習 Shell Scripts](http://linux.vbird.org/linux_basic/0340bashshell-scripts.php)

## 寫入檔案
`-p` 表示後面為提示字
最後面的 `firstname`, `lastname` 為變數名字
```shell
read -p “Please input your first name: " firstname      # 提示使用者輸入
read -p “Please input your last name:  “ lastname       # 提示使用者輸入
echo -e “\nYour full name is: ${firstname} ${lastname}” # 結果由螢幕輸出
```