---
title: 權限
comments: false
date: 2020-07-03
categories:
    - Terminal
tags:
    - terminal
    - linux
---

## 更改檔案權限
`chmod 777 <檔案名稱>`
`chmod +x <檔案名稱>`: 將每種身份都添加 execute 權限
`chmod -x <檔案名稱>`: 將每種身份都取消 execute 權限

### 權限表示
Linux檔案的基本權限就有九個，分別是owner/group/others三種身份各有自己的read/write/execute權限
第一個字:
- `-`：表示檔案
- `d`：表示資料夾

第一個字以後：
- r:4
- w:2
- x:1
每種身份的權限是累加的，所以 `chmod 777` 表示 owner/group/other **皆可以** read/write/execute

### 範例
`chmod 774 <檔案名稱>`
`-rwxr—-r--`：744
owner 可 read/write/execute = 4+2+1 = 7
group 可 read 不可 write/execute = 4 + 0 + 0 = 4
others 可 read 不可 write/execute = 4 + 0 + 0 = 4

### 執行補充
將檔案 execute 權限調整為可以後，即可以在該檔案父目錄下，在 `terminal` 輸入該檔案名字，會直接執行該檔案

#### hello.sh
```shell
echo hello world
```
#### hello.sh 的父目錄
```terminal
sh hello.sh  // hello world
chmod +x hello.sh
hello  // hello world
```


> [鳥哥的 Linux 私房菜 -- 第五章、Linux 的檔案權限與目錄配置](http://linux.vbird.org/linux_basic/0210filepermission.php)